![](screenshot.png)

# geom-builder

Simplicial-complex-like geometry builder backed by typed arrays


# Example

```sh
npm install geom-builder --save
```

```javascript
const createGeomBuilder = require('geom-builder')
const builder = createGeomBuilder({ colors: 4, cells: 3 })

builder.addPosition([0, 0, 0])
builder.addPosition([1, 0, 0])
builder.addPosition([1, 1, 0])

builder.addColor([1, 0, 0, 1])
builder.addColor([0, 1, 0, 1])
builder.addColor([0, 0, 1, 1])

builder.addCell([0, 1, 0])
// or builder.addCell(0, 1, 0)

//builder.count = 3 //vertex count
//builder.indexCount = 3 //indices count
//builder.positions = Float32Array(0, 0, 0, 1, 0, 0, 1, 1, 0, ..,)
//builder.colors = Float32Array(1, 0, 0, 1, 0, 1, 0, 1, 0, 0, 1, 1, ...)
//builder.cells = Uint16Array(0, 1, 2, ...)
```

### Running the example

```sh
cd geom-builder/example
npm install
budo index.js --open
```

# API

## GeomBuilder

```javascript
var createGeomBuilder = require('geom-builder')
```

### `builder = createGeomBuilder(opts)`

Create new geometry builder. Each attribute can be enabledby passing an integer number e.g.: `createGeomBuilder({ positions: 3, colors: 4, cells: 2 })` for colored lines.

- `opts`
    - `size` : Int - preallocated vertex buffer size, `32`
    - `positions` : Int - enable positions attribute, `3`
    - `colors` : Int - enable colors attribute
    - `normals` : Int - enable normals attribute
    - `uvs` : Int - enable uvs attribute
    - `cells` : Int - enable cells (indices)

*Note: `positions` attribute is always enabled and defaults to size 3*

## Adding vertex and index data

Every time we add vertex position, color etc internal buffer size is checked and expanded by doubling its capacity as neccesary.  Therefore `builder.count` should be used to determine how many vertices to draw instead of `builder.positions.length` as not all allocated vertex has to be used.  Similarly for meshes with cells `builder.indexCount` should be used instead of `builder.cells.length`.

All enabled attributes can be accessed by `builder.attribName` e.g.: `builder.colors`.

All data methods accept structs or individual components. E.g. `builder.addPosition(v)` and `builder.addPosition(v[0], v[1], v[2])`.

### builder.reset() 

Reset all the counters to prepare arrays for reuse. Nothing is deallocated.

### builder.addPosition(pos)

- `pos` : vec3 - position to add

*Note: adding a vertex position will increment `builder.count` counter indicanting number of vertices added so far*

### builder.addColor(color)

- `color` : vec4 - color to add

*Note: you need to enable colors in constructor*

### builder.addNormal(normal)

- `normal` : vec3 - normal to add

*Note: you need to enable normals in constructor*

### builder.addUV(uv)

- `uv` : vec2 - uv texture coord to add

*Note: you need to enable uvs in constructor*

### builder.addCell(cell)

- `cell` : number, vec2, vec3 - cell (face) with vertex indices to add

*Note: you need to enable cells in constructor*

## License

MIT, see [LICENSE.md](http://github.com/vorg/geom-builder/blob/master/LICENSE.md) for details.
