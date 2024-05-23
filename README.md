package com.example.darkmode;

import javafx.application.Application;
import javafx.beans.value.ChangeListener;
import javafx.beans.value.ObservableValue;
import javafx.geometry.Insets;
import javafx.scene.Scene;
import javafx.scene.control.*;
import javafx.scene.layout.VBox;
import javafx.scene.paint.Color;
import javafx.stage.Stage;

public class DisplaySettingsApp extends Application {

    private VBox root;
    private Label titleLabel;

    @Override
    public void start(Stage primaryStage) {
        root = new VBox(10);
        root.setPadding(new Insets(15));
        root.setStyle("-fx-background-color: #DDDDDD;");

        titleLabel = new Label("Display and Brightness");
        titleLabel.setStyle("-fx-text-fill: black; -fx-font-size: 20px;");

        ToggleGroup toggleGroup = new ToggleGroup();
        RadioButton lightMode = new RadioButton("Light");
        RadioButton darkMode = new RadioButton("Dark");
        lightMode.setToggleGroup(toggleGroup);
        darkMode.setToggleGroup(toggleGroup);
        lightMode.setSelected(true);

        toggleGroup.selectedToggleProperty().addListener((ov, old_toggle, new_toggle) -> {
            if (new_toggle != null) {
                updateTheme((RadioButton) new_toggle);
            }
        });

        Slider luminanceSlider = new Slider(0, 100, 50);
        luminanceSlider.setShowTickLabels(true);
        luminanceSlider.setShowTickMarks(true);
        luminanceSlider.setStyle("-fx-control-inner-background: #666;");
        luminanceSlider.valueProperty().addListener((obs, oldVal, newVal) -> updateBackgroundColor(newVal.doubleValue()));

        CheckBox autoAdjustment = new CheckBox("Automatic Adjustment");
        autoAdjustment.setStyle("-fx-text-fill: black;");

        root.getChildren().addAll(titleLabel, lightMode, darkMode, autoAdjustment, luminanceSlider);

        Scene scene = new Scene(root, 400, 550);
        primaryStage.setTitle("Display Settings");
        primaryStage.setScene(scene);
        primaryStage.show();
    }

    private void updateTheme(RadioButton selectedButton) {
        if ("Dark".equals(selectedButton.getText())) {
            root.setStyle("-fx-background-color: #333333;");
            titleLabel.setStyle("-fx-text-fill: white; -fx-font-size: 20px;");
        } else {
            root.setStyle("-fx-background-color: #FFFFFF;");
            titleLabel.setStyle("-fx-text-fill: black; -fx-font-size: 20px;");
        }
    }

    private void updateBackgroundColor(double brightness) {
        int colorValue = (int) (255 * (1 - brightness / 100));
        root.setStyle(String.format("-fx-background-color: rgb(%d, %d, %d);", colorValue, colorValue, colorValue));
    }

    public static void main(String[] args) {
        launch(args);
    }
}
