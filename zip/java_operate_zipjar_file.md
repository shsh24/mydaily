# Sample java program to access zip/jar file

```
import java.io.File;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.nio.file.Files;
import java.nio.file.Paths;
import java.nio.file.StandardCopyOption;
import java.util.Enumeration;
import java.util.zip.ZipEntry;
import java.util.zip.ZipException;
import java.util.zip.ZipFile;
import java.util.zip.ZipOutputStream;

public class ZipMain {
	public static void main(String[] args) {
		ZipMain m = new ZipMain();
		try {
			m.create("a.zip", "a1.txt", "a2.txt", "a3.txt");
			m.parse("a.zip");
			m.update("a.zip");
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}

	// create an zip file, with given input source
	public void create(String filename, String... inputsources) throws IOException {
		FileOutputStream fileOutputStream = new FileOutputStream(new File(filename));
		ZipOutputStream zipOutputStream = new ZipOutputStream(fileOutputStream);

		for (String inputsource : inputsources) {
			FileInputStream fileInputStream = new FileInputStream(new File(inputsource));
			ZipEntry entry = new ZipEntry(Paths.get(inputsource).getFileName().toString());

			zipOutputStream.putNextEntry(entry);
			byte[] buffer = new byte[1024];
			int bytesRead = 0;
			while ((bytesRead = fileInputStream.read(buffer)) != -1) {
				zipOutputStream.write(buffer, 0, bytesRead);
			}
			zipOutputStream.closeEntry();

			fileInputStream.close();
		}

		zipOutputStream.close();
		fileOutputStream.close();
	}

	// parse an zip file entries, and output entry content
	public void parse(String filename) throws ZipException, IOException {
		File inFile = new File(filename);
		ZipFile zipInFile = new ZipFile(inFile);

		Enumeration<ZipEntry> zipEntries = (Enumeration<ZipEntry>) zipInFile.entries();
		while (zipEntries.hasMoreElements()) {
			ZipEntry zipEntry = zipEntries.nextElement();
			System.out.println("entry: " + zipEntry.getName());

			InputStream zipEntryInputStream = zipInFile.getInputStream(zipEntry);
			byte[] buffer = new byte[1024];
			int bytesRead = 0;
			while ((bytesRead = zipEntryInputStream.read(buffer)) != -1) {
				System.out.print(new String(buffer));
			}
		}

		zipInFile.close();
	}

	// update an zip file: delete an entry, update an entry content, and add an entry
	// notice; an zip file cannot be updated directly, even append at the end; the
	// only way
	// to update an zip file is to rewrite the whole file.
	public void update(String filename) throws ZipException, IOException {
		File inFile = new File(filename);
		ZipFile zipInFile = new ZipFile(inFile);

		File outFile = File.createTempFile("tmpa", ".tmp");
		ZipOutputStream zipOutputStream = new ZipOutputStream(new FileOutputStream(outFile));
		// or in-memory if the zip file won't be too big
		// ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
		// ZipOutputStream zipOutputStream = new ZipOutputStream(outputStream);

		Enumeration<ZipEntry> jarEntries = (Enumeration<ZipEntry>) zipInFile.entries();
		while (jarEntries.hasMoreElements()) {
			ZipEntry zipEntry = jarEntries.nextElement();
			// Here zipEntry must be re-created, because it depends on compression size,
			// otherwise an exception will throw out:
			// Exception: invalid entry compressed size (expected <size> but got <size> bytes)
			zipEntry = new ZipEntry(zipEntry);
			zipEntry.setCompressedSize(-1);

			InputStream zipEntryInputStream;
			if (zipEntry.getName().endsWith("a1.txt")) {
				zipEntryInputStream = new FileInputStream(new File("b.txt"));
			} else if (zipEntry.getName().endsWith("a2.txt")) {
				// to delete a2.txt file
				continue;
			} else {
				zipEntryInputStream = zipInFile.getInputStream(zipEntry);
			}

			zipOutputStream.putNextEntry(zipEntry);
			byte[] buffer = new byte[1024];
			int bytesRead = 0;
			while ((bytesRead = zipEntryInputStream.read(buffer)) != -1) {
				zipOutputStream.write(buffer, 0, bytesRead);
			}
			zipEntryInputStream.close();
			zipOutputStream.closeEntry();
		}

		// add additional files here if necessary
		// refer to the create() process

		// close
		zipOutputStream.close();
		zipInFile.close();

		Files.move(Paths.get(outFile.getAbsolutePath()), Paths.get(inFile.getAbsolutePath()),
				StandardCopyOption.REPLACE_EXISTING);
		// or in-memory
		// FileOutputStream fileOutputStream = new FileOutputStream(inFile);
		// fileOutputStream.write(outputStream.toByteArray());
		// fileOutputStream.close();
	}
}
```

Several comments:
1. There is no way to update a zip, even just append content at the end, the only way to update an zip file is to re-generate an whole zip file.
   So the update() process is in fact to create a new zip file, and copy entries one-by-one from old zip file.
2. The general process to update an zip file is to create a temporary file, and write all contents into that file, and finally move the temporary file to target zip file.
   And if the zip file isn't too big, the temporary file can be kept in memory, and then output to disk when finish.
3. This sample can also be used to handle jar file, because jar file has same API with zip, except change ZipXXX functions/types to JarXXX functions/types.
