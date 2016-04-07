title: OutputStream源码分析
date: 2016-04-07 17:27:24
tags:
- java api
---

OutputSream是一个抽象类，属于字节流的写。
```java
import java.io.Closeable;
import java.io.Flushable;
import java.io.IOException;
 
/**
 * 该抽象类是所有字节输出流的超类。一个输出流接受输出字节并将它们发送到池里(sink)
 * 定义OutputStream的子类至少要实现写一个字节到输出流这个方法。
 *
 */
public abstract class OutputStream implements Closeable, Flushable {
    /**
     * 将特定的字节写到输出流。一般是将一个字节写到输出流。要写入的字节是参数b的低8位，高24位将被忽略。
     *
     * @param      b   the <code>byte</code>.
     * @exception  IOException  if an I/O error occurs. In particular,
     *             an <code>IOException</code> may be thrown if the
     *             output stream has been closed.
     */
    public abstract void write(int b) throws IOException;
 
    /**
     * 从给定的字节数组写到输出流b.length个字节。
     *
     * @param      b   the data.
     * @exception  IOException  if an I/O error occurs.
     * @see        java.io.OutputStream#write(byte[], int, int)
     */
    public void write(byte b[]) throws IOException {
        write(b, 0, b.length);
    }
 
    /**
     * 从给定的字节数组起始偏移地址为off读取字节数目为len有序写到输出流中。
     * 第一个写入的字节是b[off]，最后一个写入的字节是b[off+len-1]。
     *
     * @param      b     the data.
     * @param      off   数据的起始偏移地址.
     * @param      len   写到输出流的字节数目.
     * @exception  IOException  if an I/O error occurs. In particular,
     *             an <code>IOException</code> is thrown if the output
     *             stream is closed.
     */
    public void write(byte b[], int off, int len) throws IOException {
        if (b == null) {
            throw new NullPointerException();
        } else if ((off < 0) || (off > b.length) || (len < 0) ||
                   ((off + len) > b.length) || ((off + len) < 0)) {
            throw new IndexOutOfBoundsException();
        } else if (len == 0) {
            return;
        }
        for (int i = 0 ; i < len ; i++) {
            write(b[off + i]);
        }
    }
 
    /**
     * 刷新输出流并强制将缓存字节写完。它不保证这些数据被真的写入物理设备。
     *
     * @exception  IOException  if an I/O error occurs.
     */
    public void flush() throws IOException {
    }
 
    /**
     * 关闭输出流并释放与其相关的系统资源。已关闭的流无法执行输出操作且不能被重新打开。
     *
     * @exception  IOException  if an I/O error occurs.
     */
    public void close() throws IOException {
    }
 
}
```
