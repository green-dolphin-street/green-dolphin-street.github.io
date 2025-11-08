# Data storage service
- Allows applications to share a storage
- Without this, multiple applications can access the storage in incompatible ways, leading to resource contention, data corruption and data loss.

# Computer data storage types
### File system
- Manages data as a file hierarchy
- Local file system: **capability of an operating system** that services the applications running on the same computer
- Distributed file system: protocol that provides **file access between networked computers**

### Block storage
- Manages data as blocks within sectors and tracks
- A file is divided into blocks
- Can be directly accessed by the operating system as a mounted drive volume

### Object storage
- Manages data as **objects** within a container unit called **bucket**
- Examples of objects
  - Videos and photos on Facebook
  - Songs on Spotify
  - Files in Dropbox
- An object is not necessarily a file. It can be a portion of a file, or simply a collection of bits and bytes that is not part of any file
- Multiple copies of a same object is stored throughout the system to enhance availability and resilience against hardware failures
- Each object has (an unlimitedly-sized) **metadata** and a (128-bit) **globally unique identifier** that can be used to access that object without knowing its physical location.
- Usage scenarios
  - Designed to handle massive amounts of **unstructured data**
  - Adequate for storing and fetching highly available and highly durable data
  - Adequate for fast-growing database whose infrastructure is frequently scaled out, since it does not require reorganization of data when the number of storage nodes increases
  - Inadequate for frequent updates, since it doesn’t provide you with the ability to incrementally edit part(s) of a file
  - Cannot be directly accessed by the operating system as a mounted drive volume, at least without significant degradation to performance

### References
- [Object storage vs block storage ](https://www.druva.com/blog/object-storage-versus-block-storage-understanding-technology-differences)
- [Object storage Wikipedia](https://en.wikipedia.org/wiki/Object_storage)
- [File system Wikipedia](https://en.wikipedia.org/wiki/File_system)
