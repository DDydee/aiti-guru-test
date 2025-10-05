# aiti-guru-test
# Даталогическая схема данных

## Таблицы и структура

### Таблица: nomenclature
- **id**: INTEGER, PRIMARY KEY
- **id_catalog**: INTEGER, FOREIGN KEY (ссылается на catalog(id))
- **title**: VARCHAR, название номенклатуры
- **count**: INTEGER, количество
- **price**: DECIMAL, цена

### Таблица: catalog
- **id**: INTEGER, PRIMARY KEY
- **title**: VARCHAR, название каталога
- **path**: VARCHAR, путь к каталогу

### Таблица: client
- **id**: INTEGER, PRIMARY KEY
- **name**: VARCHAR, имя клиента
- **address**: VARCHAR, адрес клиента

### Таблица: orders
- **id_client**: INTEGER, FOREIGN KEY (ссылается на client(id))
- **id_nomenclature**: INTEGER, FOREIGN KEY (ссылается на nomenclature(id))

## Связи
- **nomenclature.id_catalog** → **catalog.id** (один ко многим)
- **orders.id_client** → **client.id** (многие ко многим)
- **orders.id_nomenclature** → **nomenclature.id** (многие ко многим)


## Схема таблиц

<img width="522" height="346" alt="image" src="https://github.com/user-attachments/assets/f3b33fd0-831f-475d-873d-b696a336a3bd" />


## SQL запросы
### Получение информации о сумме товаров заказанных под каждого клиента (Наименование клиента, сумма)
```sql
select client.name, sum(nomenclature.price) from orders
join nomenclature on orders.id_nomenclature = nomenclature.id
join client on orders.id_client = client.id
group by client.name
```

### Найти количество дочерних элементов первого уровня вложенности для категорий номенклатуры
```sql
select c1.title, c1.path, COUNT(c2.path)
from catalog c1
left join catalog c2 ON c2.path ~ (ltree2text(c1.path) || '.*{1}')::lquery
group by c1.title, c1.path
```
