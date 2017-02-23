# d4a4

2. Explain the importance and usage of the below terms in details
● DFSInputStream
● DFSOutputStream
● FSDataInputStream
● FSDataOutputStream

Answer:

1.DFSInputStream










2.DFSOutputStream










3.FSDataInputStream

The open() method on FileSystem actually returns an FSDataInputStream rather than a standard java.io class. This class is a specialization of java.io.DataInputStream withsupport for random access, so you can read from any part of the stream:

package org.apache.hadoop.fs;

public class FSDataInputStream extends DataInputStream

implements Seekable, PositionedReadable {
  //implementation elied
}

The Seekable interface permits seeking to a position in the file and provides a query method for the current offset from the start of the file (getPos()):

public interface Seekable {

void seek(long pos) throws IOException;

long getPos() throws IOException;

}

Calling seek() with a position that is greater than the length of the file will result in an IOException. Unlike the skip() method of java.io.InputStream, which positions the stream at a point later than the current position, seek() can move to an arbitrary, absolute
position in the file.



4.FSDataOutputStream

The create() method on FileSystem returns an FSDataOutputStream, which, like FSDataInputStream, has a method for querying the current position in the file:

package org.apache.hadoop.fs;

public class FSDataOutputStream extends DataOutputStream implements Syncable {

public long getPos() throws IOException {

// implementation elided

}

// implementation elided

}


However, unlike FSDataInputStream, FSDataOutputStream does not permit seeking. This is because HDFS allows only sequential writes to an open file or appends to an already written file. In other words, there is no support for writing to anywhere other than the end of the file, so there is no value in being able to seek while writing.
