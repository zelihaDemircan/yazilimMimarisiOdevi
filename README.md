# Yazılım Mimarisi ve Tasarımı

**Interpreter Tasarım Deseni**

Interpreter tasarım deseni, behavior grubununa ait, belli bir düzen veya kurala bağlı olan metinlerin sayısal veya mantıksal olarak işlenmesi gereken durumlarda kullanılır.Interpreter tasarım deseni, düzgün gramer ifadesindeki metinlerin sayısal veya mantıksal olarak işlenmesi gereken durumlarda kullanılır. Düzgün gramer ifadesi olarak, metinde ki karakterlerin özel karşılıkları olduğu metinler olarak düşünebiliriz.Verilen cümleye göre sayıyı ikili veya onaltılı sayıya çeviren programı örnek olarak verebiliriz.

İlk olarak yorumu yapan interpreter context sınıfını yazmalıyız.

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

Sonra interpreter context sınıfını tüketecek farklı ifade türlerini oluşturmalıyız.

```java
public interface Expression {

	String interpret(InterpreterContext ic);
}
```

İkili ve onaltılı sisteme çevirmek için iki class oluşturmalıyız.

```java
public class IntToBinaryExpression implements Expression {

	private int i;
	
	public IntToBinaryExpression(int c){
		this.i=c;
	}
	@Override
	public String interpret(InterpreterContext ic) {
		return ic.getBinaryFormat(this.i);
	}
}

public class IntToHexExpression implements Expression {

	private int i;
	
	public IntToHexExpression(int c){
		this.i=c;
	}
	
	@Override
	public String interpret(InterpreterContext ic) {
		return ic.getHexadecimalFormat(i);
	}

}
```

Kullanıcı girdisini ayrıştırmak ve doğru ifadeye iletmek için mantığa sahip olacak istemci uygulamamızı oluşturmalı ve daha sonra çıktıyı kullanıcı yanıtını oluşturmak için kullanmalıyız.

```java
public class InterpreterClient {

	public InterpreterContext ic;
	
	public InterpreterClient(InterpreterContext i){
		this.ic=i;
	}
	
	public String interpret(String str){
		Expression exp = null;
		//create rules for expressions
		if(str.contains("Hexadecimal")){
			exp=new IntToHexExpression(Integer.parseInt(str.substring(0,str.indexOf(" "))));
		}else if(str.contains("Binary")){
			exp=new IntToBinaryExpression(Integer.parseInt(str.substring(0,str.indexOf(" "))));
		}else return str;
		
		return exp.interpret(ic);
	}
	
	public static void main(String args[]){
		String str1 = "28 in Binary";
		String str2 = "28 in Hexadecimal";
		
		InterpreterClient ec = new InterpreterClient(new InterpreterContext());
		System.out.println(str1+"= "+ec.interpret(str1));
		System.out.println(str2+"= "+ec.interpret(str2));
	}
}
```

**Composite Tasarım Deseni**

Composite tasarım deseni structual grubuna ait; ağaç yapısında ki nesne kalıplarının hiyerarşik olarak iç içe kullanılmasını düzenlemektir.Hiyerarşik yapıya örnek olarak askeriyeyi verebiliriz. Rütbeli de olsa er de olsa hepsi askerdir. Rütbeli olup emrinde askerler de olsa ondan üst rütbedeki askerlerin emrindedir. Ve bir üst altına emir verdiğinde emir verilen askerin altındaki askerler de bu emirden etkilenir.Şekli verilen renkle çizmek için bir draw yöntemiyle (String fillColor) bir sınıf Shape oluşturan programı örnek olarak verebiliriz.

```java
public interface Shape {
	
	public void draw(String fillColor);
}
```

Kompozit tasarım deseni yaprağı taban bileşenini uygular ve bunlar kompozit için yapı taşıdır. Triangle, Circle gibi birden fazla yaprak nesnesi oluşturabiliriz.

```java
public class Triangle implements Shape {

	@Override
	public void draw(String fillColor) {
		System.out.println("Drawing Triangle with color "+fillColor);
	}

}
public class Circle implements Shape {

	@Override
	public void draw(String fillColor) {
		System.out.println("Drawing Circle with color "+fillColor);
	}

}
```
Kompozit bir nesne, yaprak nesneleri grubunu içerir ve gruptan yapraklar eklemek veya silmek için bazı yardımcı yöntemler sağlamalıyız.Tüm öğeleri gruptan kaldırmak için bir yöntem de sağlayabiliriz.

```java
public class Drawing implements Shape{

	//collection of Shapes
	private List<Shape> shapes = new ArrayList<Shape>();
	
	@Override
	public void draw(String fillColor) {
		for(Shape sh : shapes)
		{
			sh.draw(fillColor);
		}
	}
	
	//adding shape to drawing
	public void add(Shape s){
		this.shapes.add(s);
	}
	
	//removing shape from drawing
	public void remove(Shape s){
		shapes.remove(s);
	}
	
	//removing all the shapes
	public void clear(){
		System.out.println("Clearing all the shapes from drawing");
		this.shapes.clear();
	}
}
```
Programın çalışması için gerekli olan asıl class aşağıdaki gibi olmalı.

```java
import com.journaldev.design.composite.Circle;
import com.journaldev.design.composite.Drawing;
import com.journaldev.design.composite.Shape;
import com.journaldev.design.composite.Triangle;

public class TestCompositePattern {

	public static void main(String[] args) {
		Shape tri = new Triangle();
		Shape tri1 = new Triangle();
		Shape cir = new Circle();
		
		Drawing drawing = new Drawing();
		drawing.add(tri1);
		drawing.add(tri1);
		drawing.add(cir);
		
		drawing.draw("Red");
		
		drawing.clear();
		
		drawing.add(tri);
		drawing.add(cir);
		drawing.draw("Green");
	}

}
```


