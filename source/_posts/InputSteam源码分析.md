title: InputSteam源码分析
date: 2016-04-07 16:41:38
tags:
- java api
---
InputStream是一个抽象类，属于字节流的读取。

```java
/**
 * 该抽象类是所有字节输入流的超类。
 */
public abstract class Source implements Closeable {
 
        // 该变量用于确定在skip方法中使用的最大缓存数组大小。
    private static final int MAX_SKIP_BUFFER_SIZE = 2048;
 
    /**
     * 从输入流中读取下一字节数据。返回的字节值为一个范围在0-255之间的int数。若由于到达流的尾部而没有字节可获取，则返回-1.
     * 直到数据可达，检测到流的末尾或者抛出一个异常，该方法才停止。
     *
     * @return     下一个字节，若到达输入流结尾，则返回-1.
     * @exception  IOException  if an I/O error occurs.
     */
    public abstract int read() throws IOException;
 
    /**
     * 从输入流中读取一些字节并将它们存储到缓存数组b中。
     * 若b的长度为0，则没有字节被读取，返回0；若由于流在文件的尾部，没有字节可读，则返回-1。
     * 读取的第一个字节保存到b[0]，下一个保存到b[1]，以此类推。读取字节的数组小于等于数组b的长度。
     * 
     * @param      b   缓存所读取的数据
     * @return     读取字节数到缓存的数目。由于到达流的结尾而不可读取数据时，返回-1.
     * @exception  IOException  若输入流已经关闭或者其他I/O错误发生所导致第一个字节不能被读取而抛出的异常。不考虑到达文件尾部的情况。
     * @exception  NullPointerException  b==null的情况
     * @see        java.io.InputStream#read(byte[], int, int)
     */
    public int read(byte b[]) throws IOException {
        return read(b, 0, b.length);
    }
 
    /**
     * 从输入流读取len个字节数据，并保存到数组中。读取的数组最多为len。真正读取的字节数目将作为返回值返回。
     * 若len=0，则没有字节需要读取，返回为0；若字节不可达，则返回-1。
     * 读取到的第一个字节保存到b[off]中，下一个保存在b[off+1]，以此类推。
     *
     * @param      b     缓存所读取的数据
     * @param      off   将读取的数据写入数组b的起始偏移地址
     * @param      len   读取的最大字节数组
     * @return     保存到buffer中真正的字节数，若由于到达流的结尾而无法读取数据，返回-1.
     * @exception  IOException If the first byte cannot be read for any reason
     * other than end of file, or if the input stream has been closed, or if
     * some other I/O error occurs.
     * @exception  NullPointerException b==null的情况.
     * @exception  IndexOutOfBoundsException 若off或len为负，或者len>b.length-off时，抛出该异常。
     * @see        java.io.InputStream#read()
     */
    public int read(byte b[], int off, int len) throws IOException {
        if (b == null) {
            throw new NullPointerException();
        } else if (off < 0 || len < 0 || len > b.length - off) {
            throw new IndexOutOfBoundsException();
        } else if (len == 0) {
            return 0;
        }
 
        int c = read();
        if (c == -1) {
            return -1;
        }
        b[off] = (byte)c;
 
        int i = 1;
        try {
            for (; i < len ; i++) {
                c = read();
                if (c == -1) {
                    break;
                }
                b[off + i] = (byte)c;
            }
        } catch (IOException ee) {
        }
        return i;
    }
 
    /**
     * 跳过输入流的n个字节的数据，并返回真正跳过的字节数目。若n为负，则没有字节被跳过。
     * 方法skip会创建一个字节数组，不断读取字节到该数组中，直到n个字节被读取或到达输入流的结尾。
     *
     * @param      n   被跳过的字节数目.
     * @return     真正被跳过的字节数目.
     * @exception  IOException  if the stream does not support seek,
     *                          or if some other I/O error occurs.
     */
    public long skip(long n) throws IOException {
 
        long remaining = n;
        int nr;
 
        if (n <= 0) {
            return 0;
        }
 
        int size = (int)Math.min(MAX_SKIP_BUFFER_SIZE, remaining);
        byte[] skipBuffer = new byte[size];
        while (remaining > 0) {
            nr = read(skipBuffer, 0, (int)Math.min(size, remaining));
            if (nr < 0) {
                break;
            }
            remaining -= nr;
        }
 
        return n - remaining;
    }
 
    /**
     * 返回一个可以在该输入流中读取（或跳过）的字节数的估计，而不阻塞此输入流的下一次调用。
     *
     * @return     an estimate of the number of bytes that can be read (or skipped
     *             over) from this input stream without blocking or {@code 0} when
     *             it reaches the end of the input stream.
     * @exception  IOException if an I/O error occurs.
     */
    public int available() throws IOException {
        return 0;
    }
 
    /**
     * 关闭输入流并释放与其相关的系统资源。
     *
     *
     * @exception  IOException  if an I/O error occurs.
     */
    public void close() throws IOException {}
 
    /**
     * 标记输入流中当前的位置。当调用reset方法可以回到上次标记的位置，使得后面可以重新读取相同的字节。
     * 参数readlimit告诉输入流允许被读取的字节数目。超过该数目，则标记位失效（invalid）
     *
     * @param   readlimit   在标志位变为invalid之前，可被读取的最大字节数目.
     * @see     java.io.InputStream#reset()
     */
    public synchronized void mark(int readlimit) {}
 
    /**
     * 将流重定位到最后一次对此输入流调用mark方法时的位置。
     * 当markSupported方法返回true，则
     * 1、若mark方法自创建流以来从未被调用或者从流中读取的字节数目超过上次调用mark方法所定义的readlimit，则抛出IOException异常。
     * 2、若未抛出IOException，则将该流重新设置为状态：最近一次调用mark后（若未调用过mark，则从文件开头开始）读取的所有字节重新提供
     * 给read方法的后续调用者，如何从调用reset时起将作为下一输入数据的字节。
     * 当markSupported方法返回false，则
     * 1、调用reset方法会抛出IOException
     * 2、若IOException未被抛出，则流将被重置到一个固定状态，该状态依赖于输入流的类型和被创建的方式。。。。
     * The bytes that will be supplied to subsequent callers of the <code>read</code> method depend on the
     * particular type of the input stream. 
     *
     * @exception  IOException  若流未被标记或者标记失效。
     * @see     java.io.InputStream#mark(int)
     * @see     java.io.IOException
     */
    public synchronized void reset() throws IOException {
        throw new IOException("mark/reset not supported");
    }
 
    /**
     * 测试输入流是否支持test和reset方法。所支持的mark和reset是否是输入流实例的不变属性。
     * InputStream的markSupported方法返回false。
     *
     * @return  若流实例支持mark和reset方法，返回true；否则返回false。
     * @see     java.io.InputStream#mark(int)
     * @see     java.io.InputStream#reset()
     */
    public boolean markSupported() {
        return false;
    }
 
}
```

