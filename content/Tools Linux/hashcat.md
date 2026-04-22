

practice:

```
$apr1$oRfRsc/K$UpYpplHDlaemqseM39Ugg0
```

```

hashcat --example-hashes | grep -B 2 "apr1"

Hash mode #1600
  Name................: Apache $apr1$ MD5, md5apr1, MD5 (APR)
--
  Kernel.Type(s)......: pure, optimized
  Example.Hash.Format.: plain
  Example.Hash........: $apr1$62722340$zGjeAwVP2KwY6MtumUI1N/


```

with this, we know the mode is 1600