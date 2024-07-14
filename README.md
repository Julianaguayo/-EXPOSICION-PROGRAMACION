# EXPOSICION-PROGRAMACION

# ejecucion
![image](https://github.com/user-attachments/assets/4aba008d-3bdc-429c-b3ad-3292bbe425fc)
# ejecucion 2
![image](https://github.com/user-attachments/assets/79303bb7-9f77-470b-905f-46a158aa8726)
# ejecucion 3
![image](https://github.com/user-attachments/assets/0e4c4d01-fb0e-4a32-aa5a-0268ebec4e30)

# ESTRUCTURA DEL PROYECTO
![image](https://github.com/user-attachments/assets/058c5196-c4d6-4774-b0dd-12a8dcf95805)
Esta aplicación JavaFX permite a los usuarios ver los resultados de los constructores por año. La aplicación recupera datos de una base de datos y los muestra en una tabla. Los usuarios pueden seleccionar un año del menú desplegable para filtrar los resultados.

# Características
Selección de un año para ver los resultados de los constructores.
Mostrar nombre del constructor, victorias, puntos totales y rango en una tabla.
Actualización dinámica de la tabla según el año seleccionado.
Requisitos Previos
Antes de comenzar, asegúrate de cumplir con los siguientes requisitos:

Java Development Kit (JDK) 8 o posterior.
JavaFX SDK.
Una base de datos con las tablas y datos necesarios.
Controlador JDBC para tu base de datos (por ejemplo, MySQL, PostgreSQL).
CODIGO MAIN.JAVA
package demo_jdbc;

import java.util.List; import java.util.stream.Collectors;

import demo_jdbc.models.Constructors; import demo_jdbc.respositories.ConstructorsRepository;

import demo_jdbc.models.Season; import demo_jdbc.respositories.SeasonRepository;

import javafx.application.Application; import javafx.collections.FXCollections; import javafx.collections.ObservableList; import javafx.scene.Scene; import javafx.scene.control.ComboBox; import javafx.scene.control.TableColumn; import javafx.scene.control.TableView; import javafx.scene.control.cell.PropertyValueFactory; import javafx.scene.layout.VBox; import javafx.stage.Stage;

public class Main extends Application {

@Override
public void start(Stage primaryStage) {
    ComboBox<String> yearComboBox = createComboBox();

    TableView<Constructors> tableView = new TableView<>();

    TableColumn<Constructors, String> nameColumn = new TableColumn<>("Constructor Name");
    nameColumn.setCellValueFactory(new PropertyValueFactory<>("name"));

    TableColumn<Constructors, Integer> winsColumn = new TableColumn<>("Wins");
    winsColumn.setCellValueFactory(new PropertyValueFactory<>("wins"));

    TableColumn<Constructors, Integer> totalPointsColumn = new TableColumn<>("Total Points");
    totalPointsColumn.setCellValueFactory(new PropertyValueFactory<>("totalPoints"));

    TableColumn<Constructors, Integer> rankColumn = new TableColumn<>("Rank");
    rankColumn.setCellValueFactory(new PropertyValueFactory<>("seasonRank"));

    tableView.getColumns().add(nameColumn);
    tableView.getColumns().add(winsColumn);
    tableView.getColumns().add(totalPointsColumn);
    tableView.getColumns().add(rankColumn);

    yearComboBox.valueProperty().addListener((observable, oldValue, newValue) -> {
        if (newValue != null) {
            try {
                int selectedYear = Integer.parseInt(newValue);
                ConstructorsRepository constructorsRepository = new ConstructorsRepository();
                List<Constructors> results = constructorsRepository.getResultByYear(selectedYear);
                if (results != null) {
                    tableView.setItems(FXCollections.observableArrayList(results));
                } else {
                    System.out.println("No se encontraron resultados para el año " + selectedYear);
                }
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    });

    VBox vbox = new VBox(yearComboBox, tableView);
    Scene scene = new Scene(vbox, 400, 400);

    primaryStage.setScene(scene);
    primaryStage.setTitle("Constructor Results");
    primaryStage.show();
}

public ComboBox<String> createComboBox() {
    ComboBox<String> comboBox = new ComboBox<>();
    comboBox.setPrefSize(100, 10);

    SeasonRepository seasonRepository = new SeasonRepository();
    List<Season> years = seasonRepository.getSeasons().stream()
            .sorted((s1, s2) -> Integer.compare(s1.getYear(), s2.getYear()))
            .collect(Collectors.toList());

    for (Season season : years) {
        comboBox.getItems().add(String.valueOf(season.getYear()));
    }

    comboBox.valueProperty().addListener((observable, oldValue, newValue) -> {
        System.out.println("Ha seleccionado el año: " + newValue);
    });

    return comboBox;
}

public static void main(String[] args) {
    launch(args);
}
}

# Conclusión
Este proyecto de Visor de Resultados de Constructores demuestra cómo se puede integrar JavaFX con JDBC para crear una interfaz gráfica que permite a los usuarios interactuar con datos de una base de datos de forma intuitiva. Al seleccionar un año específico, la aplicación muestra los resultados de los constructores correspondientes, facilitando el análisis y visualización de los datos históricos.

Esperamos que esta aplicación sea útil para aprender y comprender cómo construir aplicaciones de escritorio en Java utilizando JavaFX y JDBC. Las contribuciones y sugerencias para mejorar el proyecto son siempre bienvenidas. ¡Gracias por utilizar nuestra aplicación!
