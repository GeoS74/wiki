# Резиновый макет GridPane

Задача: реализовать сетку с помощью `GridPane`, поместить внутрь какой-либо ячейки элемент `Rectangle` и сделать так, чтобы `Rectangle` заполнял собой всю ячейку при изменении размеров `GridPane`.

Решение (сетка 25х25):

```java
GridPane root = new GridPane();

// добавить столбцы
for(int i = 0; i < 25; i++){
	ColumnConstraints column = new ColumnConstraints();
	column.setPercentWidth(4);
	root.getColumnConstraints().add(column);
}
// добавить строки
for(int i = 0; i < 25; i++){
	RowConstraints row = new RowConstraints();
	row.setPercentHeight(4);
	root.getRowConstraints().add(row);
}
root.setGridLinesVisible(true);

// прямоугольник
Rectangle rect = new Rectangle(0,0,Color.RED);

// привязка размеров прямоугольника к размеру ячейки
rect.widthProperty().bind(root.widthProperty().divide(25));
rect.heightProperty().bind(root.heightProperty().divide(25));

root.add(rect, 0, 0);

// настройка растяжения GridPane
root.setHgrow(rect, Priority.ALWAYS);
root.setVgrow(rect, Priority.ALWAYS);

Scene scene = new Scene(root, 300, 250);
stage.setScene(scene);
stage.show();
```

> Прим.: здесь важно использовать `divide()` при привязке размеров прямоугольника к размерам ячейки