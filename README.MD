# patch for go/tinygo for go-fvm-sdk

## modification to fmt

remove code of os. replace os.Stdout/os.StdIn, anycode like 
```fmt.Println, fmt.Print, fmt.Printf, fmt.Sprintln, fmt.Scan, fmt.Scanf, fmt.Scanln``` have no effect

## modification to tinygo reflect

try to implement a function like reflect.Zero, but this code not accept by tinygo, just implement this function to pass build.

this code was write base on tinygo v0.24.0
```shell
func Zero(typ Type) Value {
	if typ == nil {
		panic("reflect: Zero(nil)")
	}
	t := typ.(rawType)

	switch t.Kind() {
	case Func, Chan, Map, Ptr, Slice, UnsafePointer, Interface:
		return Value{t, nil, valueFlagExported}
	case Bool, Int, Int8, Int16, Int32,Int64, Uint,Uint8,Uint16,Uint32,Uint64,Float32,
		Float64, Complex64, Complex128, String, Array, Struct:
		val := alloc(typ.Size(), nil)
		return Value{t, val, valueFlagExported&valueFlagIndirect}
	default:
		panic(&ValueError{Method: "Pointer"})
	}
}
```

## patch generate

```shell
git diff > <patch path>
```

## patch 

```shell
cd $GOPATH
patch -p1 -i fmt_v1.16_17.x.patch
```

```shell
cd $TINYGO_ROOT
patch -p1 -i /Users/lijunlong/code/go_tinygo_patch/patchs/tinygo_0.24.0_reflect.patch
```
