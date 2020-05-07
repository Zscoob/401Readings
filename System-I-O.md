# System.I.O Readings
401 class 3
## File and Stream I/O
  - File and stream I/O (input/output) refers to the transfer of data either to or from a storage medium.
  - In the .NET Framework, the System.IO namespaces contain types that enable reading and writing, both synchronously and asynchronously, on data streams and files.
     - perform compression and decompression on files
     - and types that enable communication through pipes and serial ports.
  -For path naming conventions and the ways to express a file path for Windows systems, including with the DOS device syntax supported in .NET Core 1.1 and later and the .NET Framework 4.6.2 and later
    - Here are some commonly used file and directory classes:

      - File - provides static methods for creating, copying, deleting, moving, and opening files, and helps create a FileStream object.

      - FileInfo - provides instance methods for creating, copying, deleting, moving, and opening files, and helps create a FileStream object.

      - Directory - provides static methods for creating, moving, and enumerating through directories and subdirectories.

      - DirectoryInfo - provides instance methods for creating, moving, and enumerating through directories and subdirectories.

      - Path - provides methods and properties for processing directory strings in a cross-platform manner.
## Streams
  - The Stream class and its derived classes provide a common view of data sources and repositories, and isolate the programmer from the specific details of the operating system and underlying devices.
    - Streams involve three fundamental operations:

      - Reading - transferring data from a stream into a data structure, such as an array of bytes.

      - Writing - transferring data to a stream from a data source.

      - Seeking - querying and modifying the current position within a stream.
  - Depending on the underlying data source or repository, a stream might support only some of these capabilities.
    - Here are some commonly used stream classes:

      - FileStream – for reading and writing to a file.

      - IsolatedStorageFileStream – for reading and writing to a file in isolated storage.

      - MemoryStream – for reading and writing to memory as the backing store.

      - BufferedStream – for improving performance of read and write operations.

      - NetworkStream – for reading and writing over network sockets.

      - PipeStream – for reading and writing over anonymous and named pipes.

      - CryptoStream – for linking data streams to cryptographic transformations.

## Readers and writers
  - The System.IO namespace also provides types for reading encoded characters from streams and writing them to streams
  - Each reader and writer class is associated with a stream, which can be retrieved through the class's BaseStream property.
    - Here are some commonly used reader and writer classes:

      - BinaryReader and BinaryWriter – for reading and writing primitive data types as binary values.

      - StreamReader and StreamWriter – for reading and writing characters by using an encoding value to convert the characters to and from bytes.

      - StringReader and StringWriter – for reading and writing characters to and from strings.

      - TextReader and TextWriter – serve as the abstract base classes for other readers and writers that read and write characters and strings, but not binary data.

## Asynchronous I/O operations