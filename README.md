# Proyecto-Final-Programacion-Orientada-a-Objetos

# FORMULARIO_DE_PROYECTOS_BANCO_DE_PROYECTOS_-_EVENTOS

    import javax.swing.*;
    import java.awt.*;

 * Clase que representa un formulario para la evaluación de propuestas
 * para diferentes entidades, recopilando datos personales y de entidad.

       public class Formulario extends JFrame {

// Datos personales

    private JTextField campoNombre;
    private JTextField campoEdad;
    private JTextField campoCorreo; 

// Datos de entidad

    private JComboBox<String> comboEntidad;
    private JTextField campoCapital;
    private JTextField campoPoblacion;
    private JTextField campoEventos;

    private JTextArea salida;

* Constructor que inicializa la ventana del formulario, configurando
* los campos y botones.

// Presupuesto fijo para todas las entidades

    private final double PRESUPUESTO_ASIGNADO = 10_000_000_000.0;

    public Formulario() {
        super("Sistema de Evaluación de Propuestas");

        setLayout(new BorderLayout());

        JPanel panelSuperior = new JPanel(new GridLayout(3, 1, 15, 15));

// SECCIÓN 1: DATOS PERSONALES


        JPanel panelPersonales = new JPanel(new GridLayout(3, 2, 10, 10));
        panelPersonales.setBorder(BorderFactory.createTitledBorder("Datos Personales"));

        panelPersonales.add(new JLabel("Nombre:"));
        campoNombre = new JTextField();
        panelPersonales.add(campoNombre);

        panelPersonales.add(new JLabel("Edad:"));
        campoEdad = new JTextField();
        panelPersonales.add(campoEdad);

        panelPersonales.add(new JLabel("Correo:")); 
        campoCorreo = new JTextField();
        panelPersonales.add(campoCorreo);

// SECCIÓN 2: ENTIDAD

        JPanel panelEntidad = new JPanel(new GridLayout(4, 2, 10, 10));
        panelEntidad.setBorder(BorderFactory.createTitledBorder("Entidad Elegible"));

        panelEntidad.add(new JLabel("Entidad:"));
        comboEntidad = new JComboBox<>(new String[]{
            "SENA", "Bancolombia", "Mercado Libre"
        });
        panelEntidad.add(comboEntidad);

        panelEntidad.add(new JLabel("Valor por evento (capital disponible):"));
        campoCapital = new JTextField();
        panelEntidad.add(campoCapital);

        panelEntidad.add(new JLabel("Población beneficiada:"));
        campoPoblacion = new JTextField();
        panelEntidad.add(campoPoblacion);

        panelEntidad.add(new JLabel("Número de eventos:"));
        campoEventos = new JTextField();
        panelEntidad.add(campoEventos);

        panelSuperior.add(panelPersonales);
        panelSuperior.add(panelEntidad);

        add(panelSuperior, BorderLayout.NORTH);

// BOTONES

        JPanel panelBotones = new JPanel();
        JButton botonProcesar = new JButton("Procesar");
        JButton botonEnviar = new JButton("Enviar Formulario");
        JButton botonLimpiar = new JButton("Limpiar");

        panelBotones.add(botonProcesar);
        panelBotones.add(botonEnviar);
        panelBotones.add(botonLimpiar);

        add(panelBotones, BorderLayout.CENTER);

// SALIDA

        salida = new JTextArea(15, 50);
        salida.setEditable(false);
        add(new JScrollPane(salida), BorderLayout.SOUTH);

        // Eventos
        botonProcesar.addActionListener(e -> procesarDatos());
        botonEnviar.addActionListener(e -> procesarDatos());
        botonLimpiar.addActionListener(e -> limpiarCampos());

        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setSize(600, 700);
        setLocationRelativeTo(null);
        setVisible(true);
    }
* Procesa los datos ingresados por el usuario, realiza las validaciones y muestra los resultados en la interfaz.

     
        private void procesarDatos() {
            try {
              String nombre = campoNombre.getText();
                String edad = campoEdad.getText();
                String correo = campoCorreo.getText();

            String entidad = comboEntidad.getSelectedItem().toString();
            double valorEvento = Double.parseDouble(campoCapital.getText());
            int eventos = Integer.parseInt(campoEventos.getText());
            String poblacion = campoPoblacion.getText();

            if (nombre.isEmpty() || edad.isEmpty() || correo.isEmpty() ||
                poblacion.isEmpty()) {
                salida.setText("ERROR: Todos los campos deben estar completos.");
                return;
            }

            double costoTotalEventos = valorEvento * eventos;

            boolean financiado =
                    valorEvento < PRESUPUESTO_ASIGNADO &&
                    costoTotalEventos < PRESUPUESTO_ASIGNADO;
  

            String sugerencia = obtenerSugerencia(entidad);

            StringBuilder sb = new StringBuilder();

            sb.append("========= DATOS PERSONALES =========\n");
            sb.append("Nombre: ").append(nombre).append("\n");
            sb.append("Edad: ").append(edad).append("\n");
            sb.append("Correo: ").append(correo).append("\n\n");

            sb.append("========= ENTIDAD =========\n");
            sb.append("Entidad seleccionada: ").append(entidad).append("\n");
            sb.append("Población beneficiada: ").append(poblacion).append("\n");
            sb.append("Número de eventos: ").append(eventos).append("\n");
            sb.append("Valor por evento: ").append(String.format("$%,.2f", valorEvento)).append("\n\n");

            sb.append("Presupuesto asignado: ").append(String.format("$%,.2f", PRESUPUESTO_ASIGNADO)).append("\n");
            sb.append("Costo total de eventos: ").append(String.format("$%,.2f", costoTotalEventos)).append("\n\n");

            sb.append("========= RESULTADO =========\n");
            sb.append(financiado
                    ? "La propuesta SÍ será financiada.\n"
                    : "La propuesta NO será financiada.\n");

            sb.append("\n========= SUGERENCIA =========\n");
            sb.append(sugerencia).append("\n");

            salida.setText(sb.toString());

        } catch (Exception ex) {
            salida.setText("Error: Verifica los datos ingresados.\n\n" + ex.getMessage());
        }
        }

     * Obtiene las sugerencias personalizadas para cada entidad.
     * @param entidad El nombre de la entidad seleccionada.
     * @return Las sugerencias correspondientes a la entidad.



    
            private String obtenerSugerencia(String entidad) {
            switch (entidad) {
            case "SENA":
                return "- Enfocar la propuesta en formación técnica.\n- Priorizar programas laborales.";
            case "Bancolombia":
                return "- Resaltar innovación financiera.\n- Proponer soluciones sostenibles.";
            case "Mercado Libre":
                return "- Enfocar en comercio digital.\n- Sugerir mejoras logísticas.";
            default:
                return "Sin sugerencias disponibles.";
            }
            }


* Limpia todos los campos del formulario.
    
        private void limpiarCampos() {
            campoNombre.setText("");
            campoEdad.setText("");
            campoCorreo.setText("");
            campoCapital.setText("");
            campoPoblacion.setText("");
            campoEventos.setText("");
            salida.setText("");
        }
* Método principal que ejecuta la aplicación.
* @param args Argumentos de línea de comandos.

    
        public static void main(String[] args) {
            new Formulario();
            }
        }
