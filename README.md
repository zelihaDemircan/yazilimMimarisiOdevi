# Yazılım Mimarisi ve Tasarımı

**Interpreter Tasarım Deseni**

Interpreter tasarım deseni, behavior grubununa ait, belli bir düzen veya kurala bağlı olan metinlerin sayısal veya mantıksal olarak işlenmesi gereken durumlarda kullanılır.Interpreter tasarım deseni, düzgün gramer ifadesindeki metinlerin sayısal veya mantıksal olarak işlenmesi gereken durumlarda kullanılır. Düzgün gramer ifadesi olarak, metinde ki karakterlerin özel karşılıkları olduğu metinler olarak düşünebiliriz.

```java
public class InterpreterContext {

	public String getBinaryFormat(int i){
		return Integer.toBinaryString(i);
	}
	
	public String getHexadecimalFormat(int i){
		return Integer.toHexString(i);
	}
}
```
