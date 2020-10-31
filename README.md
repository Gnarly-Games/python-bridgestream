# BridgeStream 


BridgeStream is a order oriented binary serialization library developed for performance and efficiency.

BridgeStream is used as a part of an real-time online game to
transmit data between the client and servers.

You can encode/decode primitive types, collection on primitives, custom serializers and other bridgeStream's easily
and efficiencly.


## Install

```bash
pip install bridgestream
```

## Basic Usage

Create a stream

```python
from bridgestream import BridgeStream

stream = BridgeStream()
```

Write some data

```python
stream.write_int(1)
stream.write_string("test")
stream.write_float(0.1)
stream.write_bool(True)
```

Encode

```python
data = stream.encode()
```

Decode

```python
stream = BridgeStream(data)
```

Read in order

```python
stream.read_int()  # 1
stream.read_string()  # test
stream.read_float()  # 0.1
stream.read_bool()  # True
```

## Custom Types

You can define your own serializers to abstract your common data types using `BridgeSerializer`.

You must implement the `write` method of `BridgeSerializer` 
to be able to encode it as a **stream** and you must implement the `read` method to be able to decode it as a **stream**.

```python
from bridgestream import BridgeSerializer

@dataclass  # dataclass is not required but recommended
class Vector3(BridgeSerializer):
    x: int
    y: int
    z: int

    def write(self, stream: BridgeStream):
        stream.write_int(self.x)
        stream.write_int(self.y)
        stream.write_int(self.z)

    def read(self, stream: BridgeStream):
        self.x = stream.write_int(x)
        self.y = stream.write_int(y)
        self.z = stream.write_int(z)


```

You can encode custom serializers using the `write` method of 
`BridgeStream`.

```python

vector = Vector3(1, 0, 2)

stream = BridgeStream()
stream.write(vector)

data = stream.encode()
```

You can decode a custom serializer using the `read(BirdgeSerializer)` method of 
`BridgeStream`.

```python
stream = BridgeStream(data)

vector = stream.read(Vector3)  # Vector3(x=1, y=0, z=2)
```
