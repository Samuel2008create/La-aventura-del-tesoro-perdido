import java.util.Random;
import java.util.Scanner;

public class LaAventuraDelTesoroPerdido {
    private static final int TAMANO_TABLERO = 10;
    private static final int NUMERO_OBSTACULOS = 5;
    private static final int NUMERO_ACERTIJOS = 3;
    private static final int NUMERO_OBJETOS = 4;
    private static final int NUMERO_VIDAS = 5;

    private static int[][] tablero = new int[TAMANO_TABLERO][TAMANO_TABLERO];
    private static int[] obstaculos = new int[NUMERO_OBSTACULOS];
    private static int[] acertijos = new int[NUMERO_ACERTIJOS];
    private static int[] objetos = new int[NUMERO_OBJETOS];
    private static int tesoro;
    private static int vidas = NUMERO_VIDAS;
    private static int puntuacion = 0;
    private static int posicionX = 0;
    private static int posicionY = 0;

    public static void main(String[] args) {
        // Inicializamos el tablero
        for (int i = 0; i < TAMANO_TABLERO; i++) {
            for (int j = 0; j < TAMANO_TABLERO; j++) {
                tablero[i][j] = 0;
            }
        }

        // Colocamos los obstáculos
        for (int i = 0; i < NUMERO_OBSTACULOS; i++) {
            int x = (int) (Math.random() * TAMANO_TABLERO);
            int y = (int) (Math.random() * TAMANO_TABLERO);
            tablero[x][y] = 1; // Obstáculo
            obstaculos[i] = x * TAMANO_TABLERO + y;
        }

        // Colocamos los acertijos
        for (int i = 0; i < NUMERO_ACERTIJOS; i++) {
            int x = (int) (Math.random() * TAMANO_TABLERO);
            int y = (int) (Math.random() * TAMANO_TABLERO);
            tablero[x][y] = 2; // Acertijo
            acertijos[i] = x * TAMANO_TABLERO + y;
        }

        // Colocamos los objetos
        for (int i = 0; i < NUMERO_OBJETOS; i++) {
            int x = (int) (Math.random() * TAMANO_TABLERO);
            int y = (int) (Math.random() * TAMANO_TABLERO);
            tablero[x][y] = 3; // Objeto
            objetos[i] = x * TAMANO_TABLERO + y;
        }

        // Colocamos el tesoro
        tesoro = (int) (Math.random() * TAMANO_TABLERO * TAMANO_TABLERO);

        // Inicializamos la posición del jugador
        posicionX = 0;
        posicionY = 0;

        // Juego
        while (vidas > 0) {
            // Imprimimos el tablero
            for (int i = 0; i < TAMANO_TABLERO; i++) {
                for (int j = 0; j < TAMANO_TABLERO; j++) {
                    if (i == posicionX && j == posicionY) {
                        System.out.print("J "); // Jugador
                    } else if (tablero[i][j] == 1) {
                        System.out.print("O "); // Obstáculo
                    } else if (tablero[i][j] == 2) {
                        System.out.print("A "); // Acertijo
                    } else if (tablero[i][j] == 3) {
                        System.out.print("M "); // Objeto
                    } else if (i * TAMANO_TABLERO + j == tesoro) {
                        System.out.print("T "); // Tesoro
                    } else {
                        System.out.print(". "); // Vacío
                    }
                }
                System.out.println();
            }

            // Pedimos al jugador que ingrese una dirección
            System.out.print("Ingrese una dirección (arriba, abajo, izquierda, derecha): ");
            Scanner scanner = new Scanner(System.in);
            String direccion = scanner.nextLine();

            // Movemos al jugador
            if (direccion.equals("arriba")) {
                posicionX--;
            } else if (direccion.equals("abajo")) {
                posicionX++;
            } else if (direccion.equals("izquierda")) {
                posicionY--;
            } else if (direccion.equals("derecha")) {
                posicionY++;
            }

            // Verificamos si el jugador ha llegado a un obstáculo
            if (tablero[posicionX][posicionY] == 1) {
                System.out.println("Has llegado a un obstáculo. Debes superarlo para continuar.");
                // Pedimos al jugador que ingrese una acción
                System.out.print("Ingrese una acción (saltar, luchar): ");
                String accion = scanner.nextLine();

                if (accion.equals("saltar")) {
                    // El jugador salta el obstáculo
                    tablero[posicionX][posicionY] = 0;
                    puntuacion += 10;
                } else if (accion.equals("luchar")) {
                    // El jugador lucha contra el obstáculo
                    if (Math.random() < 0.5) {
                        // El jugador gana
                        tablero[posicionX][posicionY] = 0;
                        puntuacion += 10;
                    } else {
                        // El jugador pierde
                        vidas--;
                    }
                }
            }

            // Verificamos si el jugador ha llegado a un acertijo
            else if (tablero[posicionX][posicionY] == 2) {
                System.out.println("Has llegado a un acertijo. Debes resolverlo para obtener una pista.");
                // Pedimos al jugador que ingrese una respuesta
                System.out.print("Ingrese una respuesta: ");
                String respuesta = scanner.nextLine();

                if (respuesta.equals("la respiración")) {
                    // El jugador resuelve el acertijo
                    tablero[posicionX][posicionY] = 0;
                    puntuacion += 20;
                } else {
                    // El jugador no resuelve el acertijo
                    vidas--;
                }
            }

            // Verificamos si el jugador ha llegado a un objeto
            else if (tablero[posicionX][posicionY] == 3) {
                System.out.println("Has llegado a un objeto. Puedes recogerlo y utilizarlo para ayudarte en tu aventura.");
                // Pedimos al jugador que ingrese una acción
                System.out.print("Ingrese una acción (recoger, utilizar): ");
                String accion = scanner.nextLine();

                if (accion.equals("recoger")) {
                    // El jugador recoge el objeto
                    tablero[posicionX][posicionY] = 0;
                    puntuacion += 30;
                } else if (accion.equals("utilizar")) {
                    // El jugador utiliza el objeto
                    puntuacion += 30;
                }
            }

            // Verificamos si el jugador ha llegado al tesoro
            else if (posicionX * TAMANO_TABLERO + posicionY == tesoro) {
                System.out.println("Has llegado al tesoro. ¡Felicidades!");
                puntuacion += 100;
                break;
            }
        }

        // Imprimimos la puntuación final
        System.out.println("Puntuación final: " + puntuacion);
    }
}
