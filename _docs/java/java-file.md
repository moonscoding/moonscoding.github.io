



Java IO는 파일의 속성 정보를 읽기 위해 File 제공

Java NIO는 다양한 클래스와 인터페이스 제공 - java.nio.file, java.niofile.attribute



## File

Java IO



## Path 

java.nio.file.Path Interface 

```java
Path path = Paths.get(String first, String... more);
Path path = Paths.get(URI uri);
```

- `get()`
- `compareTo()` - 경로가 동일하면 0, 상위 경로면 음수, 하위 경로면 양수, 음수와 양수 값의 차이나는 문자열 수 
  - int compareTo(Path other)
- `getFileName()` - 부모 경로를 제외한 파일 또는 디렉토리 이름만 가진 Path 반환
  - Path getFileName()
- `getFileSystem()` - FileSystem 객체 반환
  - FileSystem getFileSystem()
- `getName()` - /var/www/web, 0이면 var, 1이면 www, 2이면 web
  - Path getName(int index)
- `getNameCount()` - 중첩 경로 수 /var/www/web 이면 3 반혼
  - int getNameCount()
- `getParent()`- 바로 위 부모 폴더의 Path 반환
  - Path getParent()
- `getRoot()` - 루트 디렉토리 Path 리턴
  - Path getRoot()
- `iterator()` - 경로에 있는 모든 디렉토리와 파일을 Path 객체로 생성하고 반복자 반환
  - Iterator\<Path> iterator()
- `nomalize()` - 상대 경로로 표기할 때 불필요한 요소 제거
  - Path normalize()
- `register()` - WatchService 등록 
  - WatchKey register(...)
- `toFile()` - java.io.File 객체 반환
  - File toFile()
- `toString()` - 파일 경로 문자열 반환
  - String toString()
- `toUri()` - 파일 경로 URI 객체 반환
  - URI toUri()



## FileSystem

OS의 파일 시스템은 FileSystem 인터페이스를 통해 접근 가능

```java
FileSystem fileSystem = FileSystems.getDefault();
```

- `getFileStores()` - 드라이버 정보를 가진 FileStore 객체 목록 반환
  - Iterable\<FileStore> getFileStores()
- `getRootDirectories()` - 루프 디렉토리 정보를 가진 Path 객체 목록 반환
  - Iterable\<Path> getRootDirectories()
- `getSeparator()` - 디렉토리 구분자 반환
  - String getSeparator() 



### FileStore

- `getTotalSpace()` - 드라이버 전체 공간 크기 (byte) 반환
  - long getTotalSpace()
- `getUnallocatedSpace()` - 할당되지 않은 공간 크기 (byte) 반환
  - long getUnallocatedSpace()
- `getUsableSpace()` - 사용 가능한 공간 크기 ( = getUnallocatedSpace() )
  - long getUsableSpace() 
- `isReadOnly()` - 읽기 전용 여부
  - boolean isReadOnly()
- `name()` - 드라이버명 반환
  - String name()
- `type()` - 파일 시스템 종류
  - String type()



## Files

java.nio.file.Files Class



### Files API

- `size()` - 파일 크기 반환
  - long size()
- `readAllBytes()`
  - byte[] readAllBytes()
- `readAllLines()`
  - List\<String> readAllLines()
- `write()` - 파일에 바이트나 문자열 저장
  - Path write()
- `move()`
  - Path move()

- `copy()`
  - long copy()
  - Path copy()
- `createDirectories()`
  - Path createDirectories()
- `createDirectory()`
  - Path createDirectory()
- `createFile()`
  - Path createFile()
- `delete()`
  - void delete()
- `deleteIfExists()` - 존재하면 삭제
  - boolean deleteIfExists()
- `getFileStore()` - 파일이 위치한 FileStore(드라이브) 반환
  - FileStore getFileStore()
- `getLastModifiedTime()` - 마지막 수정 시간 반환
  - FileTime getLastModifiedTime()
- `getOwner()` - 소유자 정보 반환
  - UserPrincipal getOwner()
- `probeContentType()` - 파일의 MIME 타입 반환
  - String probeContentType()



### Files API (is)

- `exists()`
  - boolean exists()
- `notExists()`
  - boolean notExists()

- `isDirectory()`
  - boolean isDirectory()
- `isExecutable()`
  - boolean isExecutable()
- `isHidden()`
  - boolean isHidden()
- `isReadable()`
  - boolean isReadable()
- `isWritable()`
  - boolean isWritable()
- `isRegularFile()`
  - boolean isRegularFile()
- `isSameFile()`
  - boolean isSameFile()



### Files API (I/O)



- `newBufferedReader()`
  - BufferedReader newBufferedReader()
- `newBufferedWriter()`
  - BufferedWriter newBufferedWriter()
- `newByteChannel()` - 파일에 읽고 쓰는 바이트 채널을 반환
  - SeekableByteChannel newByteChannel()
- `newDirectoryStream()` - 디레토리의 모든 내용을 스트림으로 반환
  - DirectoryStream\<Path> newDirectoryStream()
- `newInputStream()`
  - InputStream newInputStream()
- `newOutputStream()`
  - OutputStream newOutputStream()



## WatchService

디렉토리 내부에서 파일 생성, 삭제, 수정 등의 내용 변화를 감지하는데 사용 (Java7)

파일 수정중에 에디터 외부에서 파일 내용을 수정하게 되면 파일 내용이 변경되어 다시 불러올 것인지 묻는 대화상자를 호출

```java
WatchService watchService = FileSystems.getDefault().newWatchService();
```



## properties

