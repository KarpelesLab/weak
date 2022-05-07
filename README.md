[![GoDoc](https://godoc.org/github.com/KarpelesLab/weak?status.svg)](https://godoc.org/github.com/KarpelesLab/weak)

# weakref map in go 1.18

This is a weakref map for Go 1.18, with some inspiration from [xeus2001's weakref implementation](https://github.com/xeus2001/go-weak).

This provides both a `weak.Ref` object to store weak references, and a `weak.Map` object for maps.

## Usage

```go
m := weak.NewMap[uint64, Object]()

// instanciate/get an object
func Get(id uint64) *Object {
	// try to get from cache
	if obj := m.Get(id); obj != nil {
		return obj
	}

	// create new
	obj := &Object{}
	obj = m.Set(id, obj) // this will return an existing object if already existing
	obj.initOnce.Do(obj.init) // use sync.Once to ensure init happens only once
	return obj
}

obj = Get(1234)
// ...
```
