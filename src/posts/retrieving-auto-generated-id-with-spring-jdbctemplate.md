---
title: Retrieving Auto Generated Id With Spring JdbcTemplate
date: 2021-03-18
tags: [java, spring, jdbc]
---

![](/images/keys.jpg)

## Introduction

When developing database applications, one of the common requirements is to get the auto generated key back from the database after an insert statement has been executed. Different database vendors offer specific commands that can be used to retrieve auto generated keys, for example in MySQL, executing the following would obtain the last inserted key value.

```sql
SELECT LAST_INSERT_ID();
```

<!-- more -->

## Using a GeneratedKeyHolder

Spring’s `JdbcTemplate` uses a `GeneratedKeyHolder` class to return the last inserted generated key when executing update commands via `PreparedStatement`s.

### Example Code

As an example, take the following `CustomerRepostory` class that implements persistence via `JdbcTemplate`s

```java
@Repository
public class CustomerRepository {

  private final static String INSERT_SQL = "insert into CUSTOMER (NAME) values (?)";
  private final Logger log = LoggerFactory.getLogger(this.getClass());

  @Autowired
  JdbcTemplate jdbcTemplate;

  public long createCustomer(String name) {
    KeyHolder key = new GeneratedKeyHolder();
    jdbcTemplate.update(new PreparedStatementCreator() {

      @Override
      public PreparedStatement createPreparedStatement(Connection connection) throws SQLException {
        final PreparedStatement ps = connection.prepareStatement(INSERT_SQL,
            Statement.RETURN_GENERATED_KEYS);
        ps.setString(1, name);
        return ps;
      }
    }, key);

    log.info("Saved customer {} with id {}.", name, key.getKey());

    return key.getKey().longValue();
  }
}
```

In this example, the `PreparedStatement` is created passing in the parameter `Statement.RETURN_GENERATED_KEYS` causing the key to be returned into the key variable.

### Example Test Code

To test this code, consider the following simple test case.

```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class CustomerRepositoryTests {

  @Autowired
  CustomerRepository customerRepository;

  @Test
  public void shouldBeAbleToRetrieveCustomerId() {
    long firstId = customerRepository.createCustomer("Anna");
    long secondId = customerRepository.createCustomer("George");

    assertThat(firstId, is(notNullValue()));
    assertThat(secondId, is(notNullValue()));
  }
}
```

When executed, the following is output into the log:

```text
2020-05-31 15:44:41.303  INFO 992 --- [           main] c.d.sample.pk.CustomerRepository         : Saved customer Anna with id 1.
2020-05-31 15:44:41.306  INFO 992 --- [           main] c.d.sample.pk.CustomerRepository         : Saved customer George with id 2.v
```

## Summary

In this article, we’ve seen how easy it is to retrieve the auto generated key when inserting using Spring’s `JdbcTemplate`.

## Credits

Photo by <a href="https://unsplash.com/@silas_crioco?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Silas Köhler</a> on <a href="/s/photos/key?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>
