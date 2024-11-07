import java.io.BufferedWriter;
import java.io.File;
import java.io.FileWriter;
import java.io.IOException;
import java.lang.reflect.Method;
import java.nio.file.Files;
import java.nio.file.Paths;
import java.util.ArrayList;
import java.util.List;

public class ProjectAnalyzer {

    public static void main(String[] args) {
        // Ruta del proyecto. Puedes cambiar esta ruta como desees.
        String projectPath = "ruta/a/tu/proyecto";
        String outputPath = "salida.txt";
        
        List<Class<?>> classes = new ArrayList<>();

        try (BufferedWriter writer = new BufferedWriter(new FileWriter(outputPath))) {
            Files.walk(Paths.get(projectPath))
                .filter(Files::isRegularFile)
                .filter(path -> path.toString().endsWith(".java"))
                .forEach(path -> {
                    try {
                        String className = getClassNameFromPath(path.toString(), projectPath);
                        Class<?> clazz = Class.forName(className);
                        if (!isOnlyBean(clazz)) {
                            classes.add(clazz);
                            writer.write("Clase: " + clazz.getName() + "\n");
                            for (Method method : clazz.getDeclaredMethods()) {
                                writer.write("  Método: " + method.getName() + "\n");
                            }
                        }
                    } catch (ClassNotFoundException | IOException e) {
                        System.err.println("No se pudo cargar la clase: " + e.getMessage());
                    }
                });
        } catch (IOException e) {
            System.err.println("Error escribiendo en el archivo de salida: " + e.getMessage());
        }

        System.out.println("Análisis completo. Resultados guardados en " + outputPath);
    }

    private static String getClassNameFromPath(String filePath, String projectPath) {
        String className = filePath
                .replace(projectPath + File.separator, "")
                .replace(".java", "")
                .replace(File.separator, ".");
        return className;
    }

    private static boolean isOnlyBean(Class<?> clazz) {
        Method[] methods = clazz.getDeclaredMethods();
        int getterSetterCount = 0;
        for (Method method : methods) {
            if (isGetterOrSetter(method)) {
                getterSetterCount++;
            }
        }
        return getterSetterCount == methods.length && getterSetterCount > 0;
    }

    private static boolean isGetterOrSetter(Method method) {
        String methodName = method.getName();
        if (methodName.startsWith("get") || methodName.startsWith("set")) {
            return method.getParameterCount() == 0 || method.getParameterCount() == 1;
        }
        return false;
    }
}








import java.io.BufferedWriter;
import java.io.File;
import java.io.FileWriter;
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.util.ArrayList;
import java.util.List;

public class JavaProjectAnalyzer {
    public static void main(String[] args) {
        String projectPath = "/path/to/your/java/project";
        String outputFilePath = "output.txt";

        analyzeProject(projectPath, outputFilePath);
    }

    public static void analyzeProject(String projectPath, String outputFilePath) {
        List<String> classNames = new ArrayList<>();
        List<String> methodNames = new ArrayList<>();

        try {
            Files.walk(Paths.get(projectPath))
                 .filter(Files::isRegularFile)
                 .filter(path -> path.toString().endsWith(".java"))
                 .forEach(path -> {
                     String className = getClassName(path.toString());
                     if (!isBean(className)) {
                         List<String> methods = getMethodNames(path.toString());
                         classNames.add(className);
                         methodNames.addAll(methods);
                     }
                 });

            writeToOutputFile(classNames, methodNames, outputFilePath);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    private static String getClassName(String filePath) {
        int lastSlashIndex = filePath.lastIndexOf(File.separator);
        int dotIndex = filePath.lastIndexOf(".");
        return filePath.substring(lastSlashIndex + 1, dotIndex);
    }

    private static boolean isBean(String className) {
        return className.endsWith("Bean");
    }

    private static List<String> getMethodNames(String filePath) {
        List<String> methodNames = new ArrayList<>();
        // Implement logic to extract method names from the Java file
        // and add them to the methodNames list
        return methodNames;
    }

    private static void writeToOutputFile(List<String> classNames, List<String> methodNames, String outputFilePath) {
        try (BufferedWriter writer = new BufferedWriter(new FileWriter(outputFilePath))) {
            for (int i = 0; i < classNames.size(); i++) {
                writer.write(classNames.get(i) + " - " + methodNames.get(i));
                writer.newLine();
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}







//analyzer flow
import java.io.BufferedWriter;
import java.io.File;
import java.io.FileWriter;
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class JavaProjectFlowAnalyzer {
    public static void main(String[] args) {
        String projectPath = "/path/to/your/java/project";
        String outputFilePath = "output.txt";

        analyzeProjectFlow(projectPath, outputFilePath);
    }

    public static void analyzeProjectFlow(String projectPath, String outputFilePath) {
        List<String> entryPoints = new ArrayList<>();
        List<String> flowChain = new ArrayList<>();

        try {
            Files.walk(Paths.get(projectPath))
                 .filter(Files::isRegularFile)
                 .filter(path -> path.toString().endsWith(".java"))
                 .forEach(path -> {
                     String className = getClassName(path.toString());
                     if (isEntryPoint(className)) {
                         entryPoints.add(className);
                         List<String> flow = getCallFlow(path.toString());
                         flowChain.addAll(flow);
                     }
                 });

            writeToOutputFile(entryPoints, flowChain, outputFilePath);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    private static String getClassName(String filePath) {
        int lastSlashIndex = filePath.lastIndexOf(File.separator);
        int dotIndex = filePath.lastIndexOf(".");
        return filePath.substring(lastSlashIndex + 1, dotIndex);
    }

    private static boolean isEntryPoint(String className) {
        // Implement logic to identify entry point classes
        // (e.g., Servlets, EJBs, web service classes)
        return className.endsWith("Servlet") || className.endsWith("Service") || className.endsWith("EJB");
    }

    private static List<String> getCallFlow(String filePath) {
        List<String> callFlow = new ArrayList<>();
        // Implement logic to analyze the Java file and extract the call flow
        // from the entry point to the database saving logic
        callFlow.add("EntryPointClass");
        callFlow.add("ServiceClass");
        callFlow.add("RepositoryClass");
        callFlow.add("DatabaseSaveOperation");
        return callFlow;
    }

    private static void writeToOutputFile(List<String> entryPoints, List<String> flowChain, String outputFilePath) {
        try (BufferedWriter writer = new BufferedWriter(new FileWriter(outputFilePath))) {
            writer.write("Entry Points:");
            writer.newLine();
            for (String entryPoint : entryPoints) {
                writer.write(entryPoint);
                writer.newLine();
            }

            writer.write("Call Flow:");
            writer.newLine();
            for (String step : flowChain) {
                writer.write(step);
                writer.newLine();
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}


//mismo pero chatgpt

import java.io.BufferedWriter;
import java.io.FileWriter;
import java.io.IOException;
import java.lang.annotation.Annotation;
import java.lang.reflect.Method;
import java.nio.file.Files;
import java.nio.file.Paths;
import java.util.ArrayList;
import java.util.List;

public class EntryPointAnalyzer {

    public static void main(String[] args) {
        // Cambia esta ruta al directorio raíz del proyecto Java que deseas analizar.
        String projectPath = "ruta/a/tu/proyecto";
        String outputPath = "flujo_entrada_a_bd.txt";

        List<Class<?>> entryPoints = new ArrayList<>();

        try (BufferedWriter writer = new BufferedWriter(new FileWriter(outputPath))) {
            Files.walk(Paths.get(projectPath))
                .filter(Files::isRegularFile)
                .filter(path -> path.toString().endsWith(".java"))
                .forEach(path -> {
                    try {
                        String className = getClassNameFromPath(path.toString(), projectPath);
                        Class<?> clazz = Class.forName(className);
                        if (isEntryPoint(clazz)) {
                            entryPoints.add(clazz);
                            writer.write("Punto de Inicio: " + clazz.getName() + "\n");
                            analyzeCallFlow(clazz, writer, "  ");
                        }
                    } catch (ClassNotFoundException | IOException e) {
                        System.err.println("No se pudo cargar la clase: " + e.getMessage());
                    }
                });
        } catch (IOException e) {
            System.err.println("Error escribiendo en el archivo de salida: " + e.getMessage());
        }

        System.out.println("Análisis completo. Resultados guardados en " + outputPath);
    }

    private static String getClassNameFromPath(String filePath, String projectPath) {
        String className = filePath
                .replace(projectPath + "/", "")
                .replace(".java", "")
                .replace("/", ".");
        return className;
    }

    private static boolean isEntryPoint(Class<?> clazz) {
        // Identificar si la clase es un punto de inicio
        for (Annotation annotation : clazz.getAnnotations()) {
            String annotationName = annotation.annotationType().getSimpleName();
            if (annotationName.equals("SpringBootApplication") || 
                annotationName.equals("WebService") || 
                annotationName.equals("Stateless") || 
                annotationName.equals("Stateful")) {
                return true;
            }
        }
        // Verificar si es un Servlet
        return Servlet.class.isAssignableFrom(clazz);
    }

    private static void analyzeCallFlow(Class<?> clazz, BufferedWriter writer, String indent) throws IOException {
        for (Method method : clazz.getDeclaredMethods()) {
            writer.write(indent + "Método: " + method.getName() + "\n");
            Class<?>[] parameterTypes = method.getParameterTypes();
            for (Class<?> paramType : parameterTypes) {
                writer.write(indent + "  Parámetro: " + paramType.getName() + "\n");
            }
            // Analizar llamadas adicionales en el flujo
            analyzeMethodCalls(method, writer, indent + "  ");
        }
    }

    private static void analyzeMethodCalls(Method method, BufferedWriter writer, String indent) throws IOException {
        // Aquí se realiza un análisis de dependencias.
        // NOTA: Este es un análisis simplificado que solo muestra nombres de clases relacionadas.
        Class<?>[] calledClasses = method.getParameterTypes();
        for (Class<?> calledClass : calledClasses) {
            writer.write(indent + "Llama a: " + calledClass.getName() + "\n");
            // Si es una llamada a base de datos
            if (isDatabaseOperation(calledClass)) {
                writer.write(indent + "  --> Operación en Base de Datos: " + calledClass.getName() + "\n");
            }
        }
    }

    private static boolean isDatabaseOperation(Class<?> clazz) {
        // Identificar si una clase representa una operación de base de datos
        // Verificar si es una clase de repositorio o de servicio de persistencia
        String className = clazz.getSimpleName().toLowerCase();
        return className.contains("repository") || className.contains("dao") || className.contains("jpa") || className.contains("database");
    }
}




//extractor y depurador
import java.io.BufferedWriter;
import java.io.File;
import java.io.FileWriter;
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.util.ArrayList;
import java.util.List;

public class JavaProjectAnalyzer {
    public static void main(String[] args) {
        String projectPath = "/path/to/your/java/project";
        String outputFilePath = "Analisis.txt";

        analyzeProject(projectPath, outputFilePath);
    }

    public static void analyzeProject(String projectPath, String outputFilePath) {
        List<String> classContents = new ArrayList<>();

        try {
            Files.walk(Paths.get(projectPath))
                 .filter(Files::isRegularFile)
                 .filter(path -> path.toString().endsWith(".java"))
                 .forEach(path -> {
                     String className = getClassName(path.toString());
                     if (!isBean(className)) {
                         String classContent = readClassContent(path.toString());
                         classContents.add(classContent);
                     }
                 });

            writeToOutputFile(classContents, outputFilePath);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    private static String getClassName(String filePath) {
        int lastSlashIndex = filePath.lastIndexOf(File.separator);
        int dotIndex = filePath.lastIndexOf(".");
        return filePath.substring(lastSlashIndex + 1, dotIndex);
    }

    private static boolean isBean(String className) {
        // Implement logic to check if a class is a bean (only has getters and setters)
        return className.endsWith("Bean");
    }

    private static String readClassContent(String filePath) {
        try {
            return new String(Files.readAllBytes(Paths.get(filePath)));
        } catch (IOException e) {
            e.printStackTrace();
            return "";
        }
    }

    private static void writeToOutputFile(List<String> classContents, String outputFilePath) {
        try (BufferedWriter writer = new BufferedWriter(new FileWriter(outputFilePath))) {
            for (String classContent : classContents) {
                writer.write(classContent);
                writer.newLine();
                writer.newLine();
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}



//caza EJB
import java.io.BufferedWriter;
import java.io.File;
import java.io.FileWriter;
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.util.ArrayList;
import java.util.List;
import java.util.stream.Collectors;

public class ProjectWSDLScanner {

    public static void main(String[] args) {
        String projectPath = "/ruta/a/tu/proyecto/java"; // Remplaza con la ruta de tu proyecto Java
        String outputFile = "outputwsdl.txt";

        List<String> wsdlFiles = new ArrayList<>();
        List<String> sqlQueries = new ArrayList<>();
        List<String> ejbCalls = new ArrayList<>();

        scanProjectForWSDLAndSQL(Paths.get(projectPath), wsdlFiles, sqlQueries, ejbCalls);

        writeOutputFile(outputFile, wsdlFiles, sqlQueries, ejbCalls);
    }

    private static void scanProjectForWSDLAndSQL(Path path, List<String> wsdlFiles, List<String> sqlQueries, List<String> ejbCalls) {
        try {
            Files.walk(path)
                 .filter(Files::isRegularFile)
                 .forEach(file -> {
                     String fileName = file.getFileName().toString();
                     String fileContent = readFileContent(file);

                     if (fileName.endsWith(".wsdl")) {
                         wsdlFiles.add(file.toString());
                     } else {
                         findSQLQueries(fileContent, sqlQueries);
                         findEJBCalls(fileContent, ejbCalls);
                     }
                 });
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    private static String readFileContent(Path file) {
        try {
            return Files.readString(file);
        } catch (IOException e) {
            e.printStackTrace();
            return "";
        }
    }

    private static void findSQLQueries(String content, List<String> sqlQueries) {
        String[] lines = content.split("\n");
        for (String line : lines) {
            line = line.trim().toLowerCase();
            if (line.startsWith("insert ") || line.startsWith("select ") || line.startsWith("update ") || line.startsWith("delete ")) {
                sqlQueries.add(line);
            }
        }
    }

    private static void findEJBCalls(String content, List<String> ejbCalls) {
        // Implementa la lógica para encontrar llamadas a EJBs en el código
    }

    private static void writeOutputFile(String fileName, List<String> wsdlFiles, List<String> sqlQueries, List<String> ejbCalls) {
        try (BufferedWriter writer = new BufferedWriter(new FileWriter(fileName))) {
            writer.write("WSDL Files:");
            writer.newLine();
            for (String wsdlFile : wsdlFiles) {
                writer.write("- " + wsdlFile);
                writer.newLine();
            }

            writer.newLine();
            writer.write("SQL Queries:");
            writer.newLine();
            for (String sqlQuery : sqlQueries) {
                writer.write("- " + sqlQuery);
                writer.newLine();
            }

            writer.newLine();
            writer.write("EJB Calls:");
            writer.newLine();
            for (String ejbCall : ejbCalls) {
                writer.write("- " + ejbCall);
                writer.newLine();
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}



///////////
import java.io.BufferedWriter;
import java.io.FileWriter;
import java.io.IOException;

public class EJBSequenceDiagramGenerator {

    public static void main(String[] args) {
        String outputFile = "salidaflujo.txt";
        generateSequenceDiagram(outputFile);
    }

    private static void generateSequenceDiagram(String fileName) {
        try (BufferedWriter writer = new BufferedWriter(new FileWriter(fileName))) {
            writer.write("@startuml");
            writer.newLine();

            writer.write("actor Client");
            writer.newLine();
            writer.write("participant \"EJBFacade\" as Facade");
            writer.newLine();
            writer.write("participant \"EJBService\" as Service");
            writer.newLine();
            writer.write("participant \"EJBRepository\" as Repository");
            writer.newLine();

            writer.write("Client -> Facade: callEJBMethod()");
            writer.newLine();
            writer.write("Facade -> Service: invokeEJBService()");
            writer.newLine();
            writer.write("Service -> Repository: performEJBOperation()");
            writer.newLine();
            writer.write("Repository --> Service: result");
            writer.newLine();
            writer.write("Service --> Facade: result");
            writer.newLine();
            writer.write("Facade --> Client: result");
            writer.newLine();

            writer.write("@enduml");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}



//

import java.io.BufferedWriter;
import java.io.File;
import java.io.FileInputStream;
import java.io.FileWriter;
import java.io.IOException;
import java.io.InputStream;
import java.util.Enumeration;
import java.util.HashSet;
import java.util.Set;
import java.util.jar.JarEntry;
import java.util.jar.JarFile;
import java.util.stream.Collectors;

public class JarAnalyzer {

    public static void main(String[] args) {
        if (args.length < 1) {
            System.err.println("Por favor, proporciona la ruta al archivo .jar");
            return;
        }

        String jarPath = args[0];
        String outputFile = "salidareporte.txt";

        Set<String> jarFiles = new HashSet<>();
        Set<String> classesAndMethods = new HashSet<>();

        analyzeJar(jarPath, jarFiles, classesAndMethods);
        writeOutputFile(outputFile, jarFiles, classesAndMethods);
    }

    private static void analyzeJar(String jarPath, Set<String> jarFiles, Set<String> classesAndMethods) {
        try (JarFile jarFile = new JarFile(new File(jarPath))) {
            Enumeration<JarEntry> entries = jarFile.entries();
            while (entries.hasMoreElements()) {
                JarEntry entry = entries.nextElement();
                if (!entry.isDirectory()) {
                    String entryName = entry.getName();
                    jarFiles.add(entryName);
                    if (entryName.endsWith(".class")) {
                        String className = getClassName(entryName);
                        Set<String> methods = getMethodNames(jarFile, entry);
                        for (String method : methods) {
                            classesAndMethods.add(className + "." + method);
                        }
                    }
                }
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    private static String getClassName(String entryName) {
        return entryName.substring(0, entryName.lastIndexOf(".")).replace("/", ".");
    }

    private static Set<String> getMethodNames(JarFile jarFile, JarEntry entry) {
        // Implementa la lógica para extraer los nombres de los métodos de la clase
        // Puedes usar bibliotecas como ASM o Javassist para analizar el bytecode de la clase
        return new HashSet<>();
    }

    private static void writeOutputFile(String fileName, Set<String> jarFiles, Set<String> classesAndMethods) {
        try (BufferedWriter writer = new BufferedWriter(new FileWriter(fileName))) {
            writer.write("Archivos JAR encontrados:");
            writer.newLine();
            for (String jarFile : jarFiles) {
                writer.write("- " + jarFile);
                writer.newLine();
            }

            writer.newLine();
            writer.write("Clases y métodos encontrados:");
            writer.newLine();
            for (String classAndMethod : classesAndMethods) {
                writer.write("- " + classAndMethod);
                writer.newLine();
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}














import com.github.javaparser.JavaParser;
import com.github.javaparser.ast.CompilationUnit;
import com.github.javaparser.ast.body.ClassOrInterfaceDeclaration;
import com.github.javaparser.ast.body.MethodDeclaration;
import com.github.javaparser.ast.visitor.VoidVisitorAdapter;

import java.io.BufferedWriter;
import java.io.File;
import java.io.FileWriter;
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.util.HashSet;
import java.util.Set;
import java.util.stream.Collectors;

public class ProjectAnalyzer {

    public static void main(String[] args) {
        if (args.length < 1) {
            System.err.println("Por favor, proporciona la ruta al proyecto Java");
            return;
        }

        String projectPath = args[0];
        String outputFile = "reportesalida.txt";

        Set<String> classes = new HashSet<>();
        Set<String> methods = new HashSet<>();
        Set<String> ejbCalls = new HashSet<>();
        Set<String> sqlQueries = new HashSet<>();

        analyzeProject(Paths.get(projectPath), classes, methods, ejbCalls, sqlQueries);
        generateReport(outputFile, classes, methods, ejbCalls, sqlQueries);
    }

    private static void analyzeProject(Path path, Set<String> classes, Set<String> methods, Set<String> ejbCalls, Set<String> sqlQueries) {
        try {
            Files.walk(path)
                 .filter(Files::isRegularFile)
                 .forEach(file -> {
                     String fileName = file.getFileName().toString();
                     String fileContent = readFileContent(file);

                     if (fileName.endsWith(".java")) {
                         extractClassesAndMethods(fileContent, classes, methods);
                         findEJBCalls(fileContent, ejbCalls);
                         findSQLQueries(fileContent, sqlQueries);
                     }
                 });
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    private static String readFileContent(Path file) {
        try {
            return Files.readString(file);
        } catch (IOException e) {
            e.printStackTrace();
            return "";
        }
    }

    private static void extractClassesAndMethods(String content, Set<String> classes, Set<String> methods) {
        try {
            CompilationUnit cu = JavaParser.parse(content);
            ClassVisitor classVisitor = new ClassVisitor(classes, methods);
            classVisitor.visit(cu, null);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    private static class ClassVisitor extends VoidVisitorAdapter<Void> {
        private final Set<String> classes;
        private final Set<String> methods;

        ClassVisitor(Set<String> classes, Set<String> methods) {
            this.classes = classes;
            this.methods = methods;
        }

        @Override
        public void visit(ClassOrInterfaceDeclaration n, Void arg) {
            super.visit(n, arg);
            classes.add(n.getFullyQualifiedName().get());
            n.getMethods().forEach(method -> methods.add(n.getFullyQualifiedName().get() + "." + method.getNameAsString()));
        }
    }

    private static void findEJBCalls(String content, Set<String> ejbCalls) {
        // Implementa la lógica para encontrar llamadas a EJBs en el código
    }

    private static void findSQLQueries(String content, Set<String> sqlQueries) {
        // Implementa la lógica para encontrar consultas SQL en el código
    }

    private static void generateReport(String fileName, Set<String> classes, Set<String> methods, Set<String> ejbCalls, Set<String> sqlQueries) {
        try (BufferedWriter writer = new BufferedWriter(new FileWriter(fileName))) {
            writer.write("Reporte del proyecto:");
            writer.newLine();

            writer.write("Clases encontradas:");
            writer.newLine();
            for (String className : classes) {
                writer.write("- " + className);
                writer.newLine();
            }

            writer.newLine();
            writer.write("Métodos encontrados:");
            writer.newLine();
            for (String methodName : methods) {
                writer.write("- " + methodName);
                writer.newLine();
            }

            writer.newLine();
            writer.write("Llamadas a EJBs encontradas:");
            writer.newLine();
            for (String ejbCall : ejbCalls) {
                writer.write("- " + ejbCall);
                writer.newLine();
            }

            writer.newLine();
            writer.write("Consultas SQL encontradas:");
            writer.newLine();
            for (String sqlQuery : sqlQueries) {
                writer.write("- " + sqlQuery);
                writer.newLine();
            }

            writer.newLine();
            writer.write("Descripción del proyecto:");
            writer.newLine();
            // Implementa la lógica para generar una descripción del proyecto
            writer.write("Este proyecto es una aplicación Java que...");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}


















