---
layout: page
title: Custom interfaces
parent: cpp_manual
---

There are three interfaces that {{page.lib_name}} provides to control how it interacts with your application. These are the render interface, the system interface and the file interface.

To install a custom interface, instance your interface and install it with the appropriate `{{page.lib_ns}}::Core::Set*Interface()` before you initialise {{page.lib_name}}.

```cpp
CustomFileInterface* file_interface = new CustomFileInterface();
{{page.lib_ns}}::Core::SetFileInterface(file_interface);
```

{{page.lib_name}} will not release your custom interfaces when it is shutdown, so make sure you destroy any you create after `{{page.lib_ns}}::Core::Shutdown()` is called.

### The file interface

The file interface controls how {{page.lib_name}} opens and reads from files, such as fonts, RCSS and RML files. If you do not install a custom file interface, {{page.lib_name}} will default to using the standard C file I/O API and attempt to open files from the current working directory. If this is satisfactory for your application you will not need to provide a custom file interface.

The file interface is given in `<{{page.lib_dir}}/Core/FileInterface.h>`{:.incl}. To develop a custom file interface, create a class derived from {{page.lib_ns}}::Core::FileInterface and provide function definitions for the pure virtual functions:

```cpp
// Opens a file.
virtual {{page.lib_ns}}::Core::FileHandle Open(const {{page.lib_ns}}::Core::String& path) = 0;

// Closes a previously opened file.
virtual void Close({{page.lib_ns}}::Core::FileHandle file) = 0;

// Reads data from a previously opened file.
virtual size_t Read(void* buffer, size_t size, {{page.lib_ns}}::Core::FileHandle file) = 0;

// Seeks to a point in a previously opened file.
virtual bool Seek({{page.lib_ns}}::Core::FileHandle file, long offset, int origin) = 0;

// Returns the current position of the file pointer.
virtual size_t Tell({{page.lib_ns}}::Core::FileHandle file) = 0;
```

These function prototypes should be fairly self-explanatory. The `{{page.lib_ns}}::Core::FileHandle` type that is returned from the `Open()` function, and passed into the other functions, is a void pointer type. This can be any value you need to uniquely identify each opened file, however the NULL (0) value is reserved for the invalid file handle, so make sure you don't use it to represent a valid handle!

`Open()` takes the string value that was given to whatever system is opening the file; this could have been through the font database, a style sheet reference in an RML document, etc. Depending on how you've configured your file interface, it needn't be a file path. The function should return a non-NULL file handle for a successful open, or NULL if the open failed.

{{page.lib_name}} will call `Close()` when it is done reading from a previously opened file.

The `Read()` function should read size bytes from the file, starting at the current position of the file pointer, into buffer. The actual number of bytes read should be returned, and the file pointer should be incremented by the same.

`Seek()` seeks the file pointer to a given location within the file. The parameters are identical to the C function `fseek()`; origin is one of `SEEK_SET` (the beginning of the file), `SEEK_CUR` (the current position of the file pointer) or `SEEK_END` (the end of the file), and offset is an offset from the origin (measured in bytes). Return false if the seek operation failed for some reason, true otherwise.

`Tell()` should return the position of the file pointer, as an offset in bytes from the origin of the file.

### The system interface

The system interfaces controls how {{page.lib_name}} tells the time, and allows your application to translate strings and output logging messages generated from the library. {{page.lib_name}} always needs a custom system interface, however the only function you need to define is `GetElapsedTime()`, the others are optional.

The system interface is given in `<{{page.lib_dir}}/Core/SystemInterface.h>`{:.incl}. To develop a custom system interface, create a class derived from `{{page.lib_ns}}::Core::SystemInterface` and provide function definitions for the one pure virtual function:

```cpp
// Get the number of seconds elapsed since the start of the application.
virtual double GetElapsedTime() = 0;
```

Provide function definitions for the other virtual functions if required:

```cpp
// Translate the input string into the translated string.
virtual int TranslateString({{page.lib_ns}}::Core::String& translated, const {{page.lib_ns}}::Core::String& input);

// Log the specified message.
virtual bool LogMessage({{page.lib_ns}}::Core::Log::Type type, const {{page.lib_ns}}::Core::String& message);
```

The `GetElapsedTime()` function should simply return the number of seconds that have elapsed since the start of the application.

`TranslateString()` is called whenever a text element is constructed from an RML stream. This allows the application to send all text read from file through its string tables.

`input` is the raw text read from the RML. translated should be set to the final text to be given to the text element to render. The total number of changes made to the raw text should be returned; If the number is greater than 0, {{page.lib_name}} will recursively call your translate function to process any new text that was added to the stream (watchout for infinite recursion). If your translation function does all the recursion itself, you can safely return 0 on every call.

Note that the translated text can include RML tags and they will be processed as if they were in the original stream; this can be used, for example, to substitute images for certain tokens.

The `LogMessage()` function is called whenever {{page.lib_name}} generates a message. type is one of the logging type, `{{page.lib_ns}}::Core::Log::ERROR` for error messages, `{{page.lib_ns}}::Core::Log::ASSERT` for failed internal assertions (debug library only), `{{page.lib_ns}}::Core::Log::WARNING` for non-fatal warnings, or `{{page.lib_ns}}::Core::Log::INFO` for generic information messages. The message parameter is the actual message itself. The function should return true if program execution should continue, or false to generate an interrupt to break execution. This can be useful if you are running inside a debugger to see exactly what an application is doing to trigger a certain message.

### The render interface

The render interface is how {{page.lib_name}} sends its generated geometry through to the application to render. It also uses the interface to load textures from external sources and from internally generated pixel data (for font textures). Applications must install a render interface before initialising {{page.lib_name}}.

The render interface is given in `<{{page.lib_dir}}/Core/RenderInterface.h>`{:.incl}. To develop a custom render interface, create a class derived from `{{page.lib_ns}}::Core::RenderInterface` and provide function definitions for the pure virtual functions, and any of the others that you wish to provide functionality for.

#### Rendering simple geometry

If you do not provide function definitions for compiling geometry, or do not compile some geometry, {{page.lib_name}} will call the `RenderGeometry()` function with geometry to be displayed:

```cpp
// Called by {{page.lib_name}} when it wants to render geometry that the application does not wish to optimise.
virtual void RenderGeometry({{page.lib_ns}}::Core::Vertex* vertices,
                            int num_vertices,
                            int* indices,
                            int num_indices,
                            {{page.lib_ns}}::Core::TextureHandle texture,
                            const {{page.lib_ns}}::Core::Vector2f& translation) = 0;
```

All geometry from {{page.lib_name}} is given in this format of indexed triangles.
* `vertices`: an array of vertices making up the geometry; each vertex is a `{{page.lib_ns}}::Core::Vertex` type, defined in `<{{page.lib_dir}}/Core/Vertex.h>`{:.incl}.
* `num_vertices`: the number of vertices in the array; no index will be equal to or higher than this number.
* `indices`: an array of integer indices, each indexing a single vertex from the vertex array.
* `num_indices`: the total number of indices in the array; as all geometry is given in triangles, this will always be a multiple of three.
* `texture`: the application-defined handle to the texture to be applied to the geometry; this will be NULL for untextured geometry.
* `translation`: the 2D translation to be applied to the geometry.

All physical coordinates (the vertex positions and the geometry translation) are given in pixel offsets from the top-left of the current context being rendered.

Geometry is rendered through the render interface in order, so while you don't necessary have to pass the geometry through to your rendering system immediately (if you're implementing some kind of geometry aggregation for example), it should still be rendered in the order it came through.

#### Compiling and rendering geometry

{{page.lib_name}} will give you the opportunity to compile all geometry it renders into a format optimal for your rendering system before it falls back to the `RenderGeometry()` function. It uses the following functions on the render interface to do this:

```cpp
// Called by {{page.lib_name}} when it wants to compile geometry it believes will be static for the forseeable future.
virtual {{page.lib_ns}}::Core::CompiledGeometryHandle CompileGeometry({{page.lib_ns}}::Core::Vertex* vertices, int num_vertices, int* indices, int num_indices, {{page.lib_ns}}::Core::TextureHandle texture);

// Called by {{page.lib_name}} when it wants to render application-compiled geometry.
virtual void RenderCompiledGeometry({{page.lib_ns}}::Core::CompiledGeometryHandle geometry, const Core::{{page.lib_ns}}::Vector2f& translation);

// Called by {{page.lib_name}} when it wants to release application-compiled geometry.
virtual void ReleaseCompiledGeometry({{page.lib_ns}}::Core::CompiledGeometryHandle geometry);
```

{{page.lib_name}} will call `CompileGeometry()` with any geometry it renders internally to give the application the choice to compile it. It provides the geometry in the same format as into the `RenderGeometry()` function above. The returned `{{page.lib_ns}}::Core::CompiledGeometryHandle` is a void pointer type which can be any value you need to uniquely identify the compiled geometry. The NULL (0) value is reserved for the invalid handle, so be sure not to return this as a valid handle! If NULL is returned, the geometry is assumed to have not been compiled and will be rendered through `RenderGeometry()` instead. The application will only be asked once to compile each geometry.

If a compiled geometry handle is returned through `CompileGeometry()`, {{page.lib_name}} will render the geometry through the `RenderCompiledGeometry()` function. It takes geometry, the handle previously returned from `CompileGeometry()`, and translation, the offset from the top-left of the current context the geometry should be rendered at.

If the compiled geometry alters or is otherwise not needed any further, {{page.lib_name}} will call `ReleaseCompiledGeometry()` to request the application to release it.

See the DirectX sample (`/Samples/basic/directx/`{:.path}) and Ogre3D (`/Samples/basic/ogre3d/`{:.path}) for examples of render interfaces making use of compiled geometry.

#### Configuring the scissor region

{{page.lib_name}} relies on scissoring regions to clip an element's hidden content. Therefore, these two functions are required to be implemented on all render interfaces:

```cpp
// Called by {{page.lib_name}} when it wants to enable or disable scissoring to clip content.
virtual void EnableScissorRegion(bool enable) = 0;

// Called by {{page.lib_name}} when it wants to change the scissor region.
virtual void SetScissorRegion(int x, int y, int width, int height) = 0;
```

`EnableScissorRegion()` is called to enable to disable scissoring on {{page.lib_name}} geometry.

`SetScissorRegion()` is called when {{page.lib_name}} wants to define the current scissor region. The scissor region is given as a rectangle, x and y being the top-left corner (as a pixel offset from the top-left corner of the rendering context). width and height are the dimensions of the rectangle, in pixels. Until the scissor region is changed, all {{page.lib_name}} geometry should be clipped to fall within this region.

For example implementations of the scissoring functions, see the sample shell (`/Samples/shell/`{:.path}) for OpenGL or the DirectX sample (`/Samples/basic/directx/`{:.path}) for DirectX 9.

#### Generating and releasing textures

{{page.lib_name}} makes calls to the render interface to load, generate and release textures. As this functionality is required by {{page.lib_name}}, these functions must be implemented in all render interfaces.

```cpp
// Called by {{page.lib_name}} when a texture is required by the library.
virtual bool LoadTexture({{page.lib_ns}}::Core::TextureHandle& texture_handle,
                         {{page.lib_ns}}::Core::Vector2i& texture_dimensions,
                         const {{page.lib_ns}}::Core::String& source) = 0;

// Called by {{page.lib_name}} when a texture is required to be built from an internally-generated sequence of pixels.
virtual bool GenerateTexture({{page.lib_ns}}::Core::TextureHandle& texture_handle,
                             const {{page.lib_ns}}::Core::byte* source,
                             const {{page.lib_ns}}::Core::Vector2i& source_dimensions) = 0;

// Called by {{page.lib_name}} when a loaded texture is no longer required.
virtual void ReleaseTexture({{page.lib_ns}}::Core::TextureHandle texture_handle) = 0;
```

`LoadTexture()` is called when {{page.lib_name}} wants to load a texture from an external source (usually a file, but this is up to the application).
* `source`: the source name specified in the RML (for an image tag) or RCSS (for a decorator image reference).
* `source_path`: the path of the referencing document.
* `texture_handle`: a reference to a `{{page.lib_ns}}::Core::TextureHandle` type. This is a void*, and can be set to whatever you need to uniquely identify the loaded texture (except 0, which is reserved for an invalid texture).
* `texture_dimensions`: should be set to the x- and y-dimensions of the loaded texture.

If the `LoadTexture()` function succeeds in loading the texture, it should return `true`. Otherwise it should return `false`.

`GenerateTexture()` is used by the font system to convert raw pixel data to a texture. The `texture_handle` parameter is a reference to a `{{page.lib_ns}}::Core::TextureHandle` type to be set, as in `LoadTexture()`. The raw pixel data is given in source; this is an array of unsigned, 8-bit values in RGBA order. It is laid out in tightly-packed rows, so is exactly `(source_dimensions.x * source_dimensions.y * 4)` bytes in size. The source_dimensions variable is set to the dimensions of the raw texture data.

`ReleaseTexture()` is called with a texture handle once it is no longer required by {{page.lib_name}}.

See the sample shell or the DirectX sample for example OpenGL and DirectX 9 texture implementations.

#### Texel offsets

As {{page.lib_name}} generates texture coordinates, it needs to be aware of texel offsets. By default, it generates texture coordinates to be compatible with OpenGL's texturing model. If this is unsuitable, override the following functions:

```cpp
// Returns the native horizontal texel offset for the renderer.
virtual float GetHorizontalTexelOffset();

// Returns the native vertical texel offset for the renderer.
virtual float GetVerticalTexelOffset();
```

You will most likely need to do this if you are rendering using DirectX, or a third-party library using DirectX underneath. In this case, return 0.5f from both of these functions, as shown in the DirectX sample. 