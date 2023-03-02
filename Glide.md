# Glide

Glide是一个安全高效的图片加载库，注重于平滑的滚动，提供了易用的API，可扩展的图片解码通道，支持**拉取，解码和展示视频快照，图片和GIF动画**。默认情况下，Glide使用的是一个**HttpUriConnection**的栈，同时提供了与Google Valley和Square okHttp快速集成的工具库。

## 缓存机制加载流程图

### LruCathe算法

LruCathe算法使用LinkedHashMap数据结构的特性，再加上对LinkedHashMap数据操作上锁的特性。

```java
public LruCache(int maxSize) {
        if (maxSize <= 0) {
            throw new IllegalArgumentException("maxSize <= 0");
        }
        this.maxSize = maxSize;
        this.map = new LinkedHashMap<K, V>(0, 0.75f, true);
    //accessOrder：true使用LruCathe算法，即选择访问排序。
    }
```



当内存缓存机制缓存已满时，调用`trimMemory()` 删除加载次数最少的图片；

```java
public void trimMemory(int level) {
    // Engine asserts this anyway when removing resources, fail faster and consistently
    Util.assertMainThread();
    // Request managers need to be trimmed before the caches and pools, in order for the latter to
    // have the most benefit.
    synchronized (managers) {
      for (RequestManager manager : managers) {
        manager.onTrimMemory(level);
      }
    }
    // memory cache needs to be trimmed before bitmap pool to trim re-pooled Bitmaps too. See #687.
    memoryCache.trimMemory(level);
    bitmapPool.trimMemory(level);
    arrayPool.trimMemory(level);
  }
```

`DiskCathe()` 接口

```java
public interface DiskCache {
  interface Factory {
    int DEFAULT_DISK_CACHE_SIZE = 250 * 1024 * 1024;
    String DEFAULT_DISK_CACHE_DIR = "image_manager_disk_cache";
    @Nullable
    DiskCache build();
  }
  interface Writer {
    boolean write(@NonNull File file);
      //写入成功返回true
  }

  @Nullable
  File get(Key key);
  void put(Key key, Writer writer);
    
  @SuppressWarnings("unused")
  void delete(Key key);

  void clear();
}
```

