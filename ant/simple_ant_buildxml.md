# This is a simple java ant project file

Suppose the file structure is:
```
+ build.xml
+ src/com/company/product
+ -----------------------/Main.java
+ -----------------------/Function.java
```

This build.xml will build out a jar file, include the main entry.
```
<?xml version="1.0" encoding="UTF-8"?>
<project name="A simple ant project build" default="jar" basedir=".">
  <!-- Sets the properties here-->
  <property name="src.dir" location="src" />
  <property name="build.dir" location="bin" />

  <!-- Target for deleting the existing directories-->
  <target name="clean">
    <delete file="${basedir}/product.jar"/>
    <delete dir="${build.dir}" />
  </target>

  <!-- Target for creating the new directories-->
  <target name="makedir">
    <mkdir dir="${build.dir}" />
  </target>

  <!-- Target for compiling the java code-->
  <target name="compile" depends="clean, makedir">
    <javac includeantruntime="false" srcdir="${src.dir}" destdir="${build.dir}">
    </javac>
  </target>

  <!-- Default target to run all targets-->
  <target name="jar" depends="compile">
    <jar destfile="${basedir}/product.jar" basedir="${build.dir}">
      <include name="**/*.class"/>
      <manifest>
        <attribute name="Main-Class" value="com.company.product.Main"/>
      </manifest>
    </jar>
  </target>

</project>
```

After build the jar successfully, user can launch the program with:
```
java -jar product.jar
```
So that no need to identify the main entry class com.company.product.Main in command line, since it has been included into jar file as manifest information.
