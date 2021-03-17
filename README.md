# helloPostgreSQL
PostgreSQL 연습 

#### dependency 추가
> 의존성 추가
~~~xml
  		<!-- postgreSQL -->
		<dependency>
			<groupId>org.postgresql</groupId>
			<artifactId>postgresql</artifactId>
			<version>42.2.5</version>
		</dependency>
~~~

#### properties 파일

~~~
db.url=jdbc:postgresql://localhost:5432/testdb
db.user=postgres
db.passwd= "공백"
~~~
#### <사용할 SQL 문>

~~~
DROP TABLE IF EXISTS books, authors, testing, images;

CREATE TABLE IF NOT EXISTS authors (
    id serial PRIMARY KEY, 
    name VARCHAR(25)
);

CREATE TABLE IF NOT EXISTS books (
    id serial PRIMARY KEY, 
    author_id INT references authors(id), title VARCHAR(100)
);

CREATE TABLE IF NOT EXISTS testing(id INT);
CREATE TABLE IF NOT EXISTS images(id serial, data bytea);

INSERT INTO authors(id, name) VALUES(1, 'Jack London');
INSERT INTO authors(id, name) VALUES(2, 'Honore de Balzac');
INSERT INTO authors(id, name) VALUES(3, 'Lion Feuchtwanger');
INSERT INTO authors(id, name) VALUES(4, 'Emile Zola');
INSERT INTO authors(id, name) VALUES(5, 'Truman Capote');

INSERT INTO books(id, author_id, title) VALUES(1, 1, 'Call of the Wild');
INSERT INTO books(id, author_id, title) VALUES(2, 1, 'Martin Eden');
INSERT INTO books(id, author_id, title) VALUES(3, 2, 'Old Goriot');
INSERT INTO books(id, author_id, title) VALUES(4, 2, 'Cousin Bette');
INSERT INTO books(id, author_id, title) VALUES(5, 3, 'Jew Suess');
INSERT INTO books(id, author_id, title) VALUES(6, 4, 'Nana');
INSERT INTO books(id, author_id, title) VALUES(7, 4, 'The Belly of Paris');
INSERT INTO books(id, author_id, title) VALUES(8, 5, 'In Cold blood');
INSERT INTO books(id, author_id, title) VALUES(9, 5, 'Breakfast at Tiffany');

~~~

#### ex)  JavaPostgreSQLPrepared.java

~~~java
package com.zetcode;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.SQLException;
import java.util.logging.Level;
import java.util.logging.Logger;

public class JavaPostgreSQLPrepared {

	public static void main(String[] args) {

		String url = "jdbc:postgresql://localhost:5432/testdb";
		String user = "postgres";
		String password = "";

		int id = 6;
		String author = "Trygve Gulbranssen";
		String query = "INSERT INTO authors(id, name) VALUES(?, ?)";
		try (Connection con = DriverManager.getConnection(url, user, password);
				PreparedStatement pst = con.prepareStatement(query)) {

			pst.setInt(1, id);
			pst.setString(2, author);
			pst.executeUpdate();

		} catch (SQLException ex) {

			Logger lgr = Logger.getLogger(JavaPostgreSQLPrepared.class.getName());
			lgr.log(Level.SEVERE, ex.getMessage(), ex);
		}
	}
}

~~~

### 참고사이트
https://zetcode.com/java/postgresql/
