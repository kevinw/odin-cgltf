# odin-cgltf

[Odin](https://odin-lang.org/) bindings for the [cgltf](https://github.com/jkuhlmann/cgltf) model loading/writing library.

## Usage

```odin

import "core:fmt"
import "core:mem"

main :: proc() {
    options := cgltf.Options{};
    gltf:^cgltf.Data;

    path_root := "./";

    result := cgltf.parse_file(&options, "model.gltf", &gltf);
    if result != .SUCCESS {
        fmt.eprintln("error parsing gltf");
        return;
    }

    defer cgltf.free(gltf);

    // load all buffers
    buffers := mem.slice_ptr(gltf.buffers, gltf.buffers_count);
    for _, i in buffers {
        gltf_buf := &buffers[i];
        full_uri := fmt.tprintf("%s%s", path_root, gltf_buf.uri);
        ok, bytes := os.read_entire_file(full_uri);

        // init buffer in your graphics api
    }

    // ...
}

```
