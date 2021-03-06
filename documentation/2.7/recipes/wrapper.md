---
---
# Echache Wrapper

 

## Introduction
This page provides an example of a simple class to make accessing Ehcache easier for simple use cases.

## Problem

Using the full Ehcache API can be more tedious than using just a simple, value-based cache (like a HashMap) because of the Element class that holds values.

## Solution

Implement a simple cache wrapper to hide the use of the Element class.

## Discussion

Here's a simple class you can use to simplify using Ehcache in certain simple use cases.

You can still get the Ehcache cache in case you want access to the full API.

~~~ java
public interface CacheWrapper<K, V> {
  void put(K key, V value);
    
  V get(K key);
}
~~~

~~~ java    
import net.sf.ehcache.CacheManager;
import net.sf.ehcache.Ehcache;
import net.sf.ehcache.Element;
    
public class EhcacheWrapper<K, V> implements CacheWrapper<K, V> {
  private final String cacheName;
  private final CacheManager cacheManager;

  public EhcacheWrapper(final String cacheName, final CacheManager cacheManager) {
    this.cacheName = cacheName;
    this.cacheManager = cacheManager;
  }
    
  public void put(final K key, final V value) {
    getCache().put(new Element(key, value));
  }
    
  public V get(final K key) {
    Element element = getCache().get(key);
    if (element != null) {
      return (V) element.getValue();
    }
    return null;
  }
    
  public Ehcache getCache() {
    return cacheManager.getEhcache(cacheName);
  }
}
~~~
