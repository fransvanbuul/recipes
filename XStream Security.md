# XStream Security settings

Default XStream configuration gives a (rightful) security warning because it deserializes everything.

To fix:

```
@Autowired
public void config(Serializer serializer) {
    if (serializer instanceof XStreamSerializer) {
        XStream xStream = ((XStreamSerializer) serializer).getXStream();
        XStream.setupDefaultSecurity(xStream);
        xStream.allowTypesByWildcard(new String[]{
                   "com.example.**",
                   "org.axonframework.**"});
   }
}
```

