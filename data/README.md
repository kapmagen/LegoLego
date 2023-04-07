```CREATE TABLE colors (
id INTEGER PRIMARY KEY,
name VARCHAR (255),
rgb VARCHAR (255),
is_trans VARCHAR (255)
)
```

```CREATE TABLE themes (
id INTEGER PRIMARY KEY,
name VARCHAR (255),
parent_id INTEGER
)
```
```CREATE TABLE part_categories (
id INTEGER PRIMARY KEY,
name VARCHAR (255)
)
```
```CREATE TABLE parts (
part_num VARCHAR (255) PRIMARY KEY,
name VARCHAR (255),
part_cat_id INTEGER,
FOREIGN KEY(part_cat_id) REFERENCES part_categories(id)
)
```
```CREATE TABLE sets (
set_num VARCHAR (255) PRIMARY KEY,
name VARCHAR (255),
year INTEGER,
theme_id INTEGER,
num_parts INTEGER,
FOREIGN KEY(theme_id) REFERENCES themes(id)
)
```
```CREATE TABLE inventories (
id INTEGER PRIMARY KEY,
version INTEGER,
set_num VARCHAR (255),
FOREIGN KEY(set_num) REFERENCES sets(set_num)
)```

```CREATE TABLE inventory_sets (
inventory_id INTEGER,
set_num VARCHAR(255),
quantity INTEGER,
FOREIGN KEY(inventory_id) REFERENCES inventories(id),
FOREIGN KEY(set_num) REFERENCES sets(set_num)
)```

```CREATE TABLE inventory_parts (
inventory_id INTEGER,
part_num VARCHAR(255),
color_id INTEGER,
quantity INTEGER,
is_spare VARCHAR(255),
FOREIGN KEY(inventory_id) REFERENCES inventories(id),
FOREIGN KEY(color_id) REFERENCES colors(id)
)
```
