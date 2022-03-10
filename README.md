# foreign-structures
Panama project compiler that compiles C like structures to MemoryLayouts

## C/C++ like structures
The provided compiler takes C/C++ like native structures and generates `MemoryLayout` hierarchy based on the provided structures.

The compiler takes something resembling a C/C++ structure
```java
public final static String TEST_STRING = """
	struct Node1 {
		byte isLeaf;
		union U1 {
		    struct S1 {
		        struct Node1 *left;
		        struct Node1 *right;
		    }  *internal[5],
		    	external;
		    double data;
		} info;
	};
	""";
```

and turns it into the following java code equivelent

```java
public final static GroupLayout structNode1 = structLayout(
	valueLayout(8, nativeOrder()).withName("isLeaf"),
	unionLayout(
		sequenceLayout(5,
			valueLayout(64, nativeOrder()).withName("Node1.U1.S1 *")
		).withName("internal[]"),
		structLayout(
			valueLayout(64, nativeOrder()).withName("left"),
			valueLayout(64, nativeOrder()).withName("right")
		).withName("external"),
		valueLayout(64, nativeOrder()).withName("data")
	).withName("info")
);
```
