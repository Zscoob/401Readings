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