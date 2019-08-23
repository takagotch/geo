### geo
---
https://www.w3.org/TR/geolocation-API/

.java
https://github.com/davidmoten/geo/

```java
// geo/src/test/java/com/github/davidmoten/geo/db/DatabaseTest.java

public class DatabaseTest {
  
  @Test
  public void testCreateDatabaseInsertRecordsRecordsAndRunBenchmarkQueries()
      throws IOException, SQLException {
    Connenction con = createDatabase();
    
    long now = System.currentTimeMillis();
    
    insertRecords(con, now);
    
    createIndexes(con);
    
    for (int length = 2; length <= 5; length++) {
      displayQueryTimes(con, now, length);
    }
    
    displayMultipleRangeQueryTime(con, now);
    
    con.close();
  }
  
  private void displayMultipleRangeQueryTime(Connection con, long now)
      throws SQLException {
    System.out.println("----");
    System.out.println("running multiple range query");
    PreparedStatement ps = con
    
    ps.setLong(1,)
    
    
    }
}



```

---
### gis
---


```
```

