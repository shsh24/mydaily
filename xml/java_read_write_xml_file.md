# A simple java program to read and write XML file

```
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;

import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.parsers.ParserConfigurationException;
import javax.xml.transform.Transformer;
import javax.xml.transform.TransformerException;
import javax.xml.transform.TransformerFactory;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;
import javax.xml.parsers.DocumentBuilder;

import org.w3c.dom.Document;
import org.xml.sax.SAXException;
import org.w3c.dom.Element;

public class XMLMain {
	// read from xml file
	public Document readXML(String filename) throws ParserConfigurationException, SAXException, IOException {
		FileInputStream inputFile = new FileInputStream(filename);
		DocumentBuilderFactory dbFactory = DocumentBuilderFactory.newInstance();
		DocumentBuilder dBuilder = dbFactory.newDocumentBuilder();
		Document doc = dBuilder.parse(inputFile);
		return doc;
	}

	// write to xml file
	public void writeXML(Document doc, String filename) throws FileNotFoundException, TransformerException {
		FileOutputStream output = new FileOutputStream(filename);
		TransformerFactory transformerFactory = TransformerFactory.newInstance();
		Transformer transformer = transformerFactory.newTransformer();

		DOMSource source = new DOMSource(doc);
		StreamResult result = new StreamResult(output);
		transformer.transform(source, result);
	}
	
	// process doc
	public void processXML(Document doc) {
		Element root = doc.getDocumentElement();
		System.out.println("root=" + root.getNodeName());
	}
	
 	public static void main(String[] args) {
		XMLMain m = new XMLMain();
		try {
			Document doc = m.readXML("input.xml");
			m.processXML(doc);
			m.writeXML(doc, "output.xml");
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```

* Notice: there is a JDK(1.8.x) bug when handle comment nodes when outside the root node, for example:
```
<?xml version="1.0" standalone="yes" ?>
<!-- comment line -->
<root_node>
   <sub_node>
      <attr1>Tom</attr1>
      <attr2>123</attr2>
   </sub_node>
</root_node>
```
It will generate output as:
```
<?xml version="1.0" encoding="UTF-8"?><!-- comment line --><root_node>
   <sub_node>
      <attr1>Tom</attr1>
      <attr2>123</attr2>
   </sub_node>
```
The required newlines, pre and post the comment node, are lost; and if the comment nodes are inside the root node, it doesn't have this issue.
