package PROJECT_Z;

import java.util.Scanner;
import java.util.regex.Matcher;
import java.util.regex.Pattern;

/**
 * @author Oliuuer
 */
public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        GestorContactos g = new GestorContactos("","","");
        g.cargarContactos();
        Pattern patronNombre = Pattern.compile("^[A-Za-zÁÉÍÓÚáéíóúÑñ\\s]+$");
        Pattern patronTelefono = Pattern.compile("^9\\d{8}$");
        Pattern patronCorreo = Pattern.compile("^[\\w.-]+@[\\w.-]+\\.[A-Za-z]{2,}$");
        int op;
        do{
            System.out.println("_______________________");
            System.out.println("  GESTOR DE CONTACTOS  ");
            System.out.println("-----------------------");
            System.out.println("1. AGREGAR CONTACTO");
            System.out.println("2. BUSCAr CONTACTO");
            System.out.println("3. EDITAR CONTACTO");
            System.out.println("4. ELIMINAR CONTACTO");
            System.out.println("5. MOSTRAR TODOS");      
            System.out.println("0. SALIR");
            System.out.print("Elige opción: ");
            op = sc.nextInt();
            sc.nextLine(); // limpiar buffer
            switch(op){
                case 1:
                    String n;
                    boolean nombreValido;
                    do{
                        System.out.print("Nombre: ");
                        n=sc.nextLine();
                        Matcher evalNombre = patronNombre.matcher(n);
                        nombreValido= evalNombre.matches();
                        if(!nombreValido){
                            System.out.println("Un nombre no lleva numeros!");
                        }
                    }while(!nombreValido);
                    String tel;
                    boolean telValido;
                    do{
                        System.out.print("Telefono: ");
                        tel=sc.nextLine();
                        Matcher evalTel = patronTelefono.matcher(tel);
                        telValido = evalTel.matches();
                        if(!telValido){
                            System.out.println("INCORRECTO, TRY AGAIN");
                        }
                    }while(!telValido);
                    String email;
                    boolean emailValido;
                    do {
                        System.out.print("Email: ");
                        email = sc.nextLine();
                        Matcher evalEmail = patronCorreo.matcher(email);
                        emailValido = evalEmail.matches();
                        if (!emailValido) {
                            System.out.println("Correo inválido. Debe contener un '@' y tener formato correcto.");
                        }
                    } while (!emailValido);
                    g.agregar(new Contacto(n,tel,email));
                    break;
                case 2:
                    System.out.print("Nombre a buscar: ");
                    String buscar = sc.nextLine();
                    Contacto encontrado = g.buscar(buscar);
                    System.out.println(encontrado != null ? encontrado : "No encontrado");
                    break;
                case 3:
                    System.out.print("Nombre a editar: ");
                    String editar = sc.nextLine();
                    System.out.print("Nuevo Teléfono: ");
                    String nuevoTel = sc.nextLine();
                    System.out.print("Nuevo Email: ");
                    String nuevoEmail = sc.nextLine();
                    g.editar(editar, nuevoTel, nuevoEmail);
                    break;
                case 4:
                    System.out.print("Nombre a eliminar: ");
                    String eliminar = sc.nextLine();
                    g.eliminar(eliminar);
                    break;
                case 5:
                    g.mostrarTodos(); break;
                case 0:
                    g.guardarContactos();
                    System.out.println("Saliendo ...."); break;
                default:
                    System.out.println("OPCION INVALIDA");
            }
        }while(op != 0);
        sc.close();
    }
}
