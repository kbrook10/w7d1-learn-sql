## Question: (1)
***Code Example Question(1):*** *How many users are there?*
```
select count(*) from users;
```
Answer: 50

## Question: (2)
***Code Example Question(2):*** *What are the 5 most expensive items?*
```
select * from items order by price DESC limit 5;
```
Answer:
id|title|category|description|price
25|Small Cotton Gloves|Automotive, Shoes & Beauty|Multi-layered modular service-desk|9984
83|Small Wooden Computer|Health|Re-engineered fault-tolerant adapter|9859
100|Awesome Granite Pants|Toys & Books|Upgradable 24/7 access|9790
40|Sleek Wooden Hat|Music & Baby|Quality-focused heuristic info-mediaries|9390
60|Ergonomic Steel Car|Books & Outdoors|Enterprise-wide secondary firmware|9341

## Question: (3)
***Code Example Question(3):*** *What's the cheapest book? (Does that change for "category is exactly 'book'" versus "category contains 'book'?*
```
select category, title, price from items where category like '%book%';
```
Answer: Ergonimic Granite Cha at 1496, and the cheapest does not change...

category    title                  price     
----------  ---------------------  ----------
Books       Fantastic Steel Chair  9246      
Toys & Boo  Fantastic Rubber Shir  6720      
Books       Fantastic Rubber Shoe  8904      
Books & Ou  Ergonomic Steel Car    9341      
Books       Ergonomic Granite Cha  1496      
Beauty, Mo  Fantastic Plastic Glo  6584      
Beauty & B  Small Cotton Hat       1727      
Books, Toy  Incredible Granite Co  2377      
Books       Practical Plastic Hat  3056      
Toys & Boo  Awesome Granite Pants  9790

## Question: (4)
***Code Example Question(4):*** *Who lives at "6439 Zetta Hills, Willmouth, WY"? Do they have another address?*
```
select addresses.user_id, addresses.street, addresses.city, addresses.state, users.id, users.first_name, users.last_name from addresses inner join users on addresses.user_id=users.id where street='6439 Zetta Hills';
```
Answer: Corrine Little and she has another address...
```
select addresses.user_id, addresses.street, addresses.city, addresses.state, users.id, users.first_name, users.last_name from addresses inner join users on addresses.user_id=users.id;
```

user_id     street              city        state       id          first_name  last_name
----------  ------------------  ----------  ----------  ----------  ----------  ----------
40          6439 Zetta Hills    Willmouth   WY          40          Corrine     Little    
40          54369 Wolff Forges  Lake Bryon  CA          40          Corrine     Little   

## Question: (5)
***Code Example Question(5):*** *Correct Virginie Mitchell's address to "New York, NY, 10108".*
```
select addresses.user_id, addresses.city, addresses.state, addresses.zip, users.id, users.first_name, users.last_name from addresses inner join users on addresses.user_id=users.id where user_id=39;
```
Ran this code to find Virginie Mitchell's address which is id:41

id          user_id     city        state       zip         id          first_name  last_name
----------  ----------  ----------  ----------  ----------  ----------  ----------  ----------
41          39          Roxanehave  NY          34705       39          Virginie    Mitchell  
42          39          Bahringerl  WY          24028       39          Virginie    Mitchell
```
 update addresses set city='New York', state='NY', zip='10108' where id=41;
```
Ran this code to update Virginie Mitchell's address to New York, NY 10108, then reran to above code to validate.


## Question: (6)
***Code Example Question(6):*** *How much would it cost to buy one of each tool?*
```
select sum(price) as total from items where category like '%tools%';
```
Answer: 467477
total     
----------
46477

## Question: (7)
***Code Example Question(7):*** *How many total items did we sell?*
```
select sum(quantity) as items_sold from orders;
```
Answer: 2125

items_sold
----------
2125   
## Question: (8)
***Code Example Question(8):*** *How much was spent on books?*
Answer: 420566
total_books_sold
----------------
420566
```
select sum(orders.quantity * items.price) as total_books_sold from orders inner join items on orders.item_id=items.id where items.category like 'books';
```

Ran this to confirm answer...
```
select orders.id, orders.item_id, orders.quantity, items.category, items.price from orders inner join items on orders.item_id=items.id where items.category like 'books';
```
id          item_id     quantity    category    price     
----------  ----------  ----------  ----------  ----------
16          76          8           Books       1496      
42          98          7           Books       3056      
65          98          9           Books       3056      
86          21          8           Books       8904      
106         76          7           Books       1496      
140         21          6           Books       8904      
151         21          9           Books       8904      
157         21          8           Books       8904      
214         4           5           Books       9246      
307         21          2           Books       8904      
309         98          1           Books       3056      
356         98          2           Books       3056

## Question: (9)
***Code Example Question(9):*** *Simulate buying an item by inserting a User for yourself and an Order for that User.*
Create User
```
insert into users (first_name, last_name, email) values ('Keith', 'Brooks', 'kbrook10@gmail.com');
```
id          first_name  last_name   email                     
----------  ----------  ----------  --------------------------
51          Keith       Brooks      kbrook10@gmail.com

Create address
```
insert into addresses (user_id, street, city, state, zip) values ('51', '123 Mulberry Rd.', 'Indianapolis', 'IN', '46268');
sqlite> select * from addresses where user_id='51';
```
id          user_id     street            city          state       zip       
----------  ----------  ----------------  ------------  ----------  ----------
55          51          123 Mulberry Rd.  Indianapolis  IN          46268   

Create order
```
insert into orders (user_id, item_id, quantity, created_at) values (51, 99, 2, strftime("%Y-%m-%d %H:%M:%S.%s"));
```
id          user_id     item_id     quantity    created_at         
----------  ----------  ----------  ----------  -------------------
378         51          89          2           2016-11-07 20:47:26
379         51          99          1                              
380         51          99          1                              
381         51          99          1           2016-11-07 21H:02M:
382         51          99          1           2016-11-07 21:04:12
383         51          99          1           1478552789         
384         51          99          2           1478554057         
385         51          99          2           2016-11-07 21:30:49
386         51          99          2           2016-11-07 21:31:08
387         51          99          2           2016-11-07 21:31:31
388         51          99          2           2016-11-07 21:35:36

##(Adventure Mode)

## Question: (1A): What item was ordered most often?
```
select item_id,
   ...> sum(quantity) as total
   ...> from orders
   ...> group by item_id
   ...> order by total desc
   ...> limit 5;
```
Answer: Item_id 65 with a qty. total of 72

## Question: (2A): Grossed the most money?
```
select orders.item_id, orders.quantity, items.id, items.price,
   ...> sum(orders.quantity * items.price) as gross_money
   ...> from orders
   ...> inner join items
   ...> on (orders.item_id = items.id)
   ...> group by orders.item_id
   ...> order by gross_money desc
   ...> limit 5;
```
Answer: Item_id 65 with a gross money of 525240
item_id     quantity    id          price       gross_money
----------  ----------  ----------  ----------  -----------
65          10          65          7295        525240     
41          10          41          8324        449496     
90          5           90          8895        444750     
46          5           46          7534        399302     
45          1           45          7479        329076 
