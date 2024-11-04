public class WsImportExample {
    public static void main(String[] args) {
        String[][] wsImportArgs = new String[][]{
            {"-target", "2.1"},
            {"-s", "C:/Users/tuUsuario/tuProyecto/src/main/java"},
            {"-keep"},
            {"-Xnocompile"},
            {"-extension"},
            {"-encoding", "UTF-8"},
            {"-wsdllocation", "http://localhost/wsd1"},
            {"C:/Users/tuUsuario/tuProyecto/src/main/resources/META-INF/SomeService.wsdl"}
        };
        
        try {
            com.sun.tools.ws.WsImport.main(wsImportArgs);
            System.out.println("WsImport executed successfully");
        } catch (Exception e) {
            System.err.println("Error executing WsImport: " + e.getMessage());
            e.printStackTrace();
        }
    }
}
