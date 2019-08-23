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
        .prepareStatement("select name,lat,lon from report where time >= ? and time <? and lat>=-6 and lat<=-5 and,,,goehash10,oehash11,oehash12") value(?,?,?,?,?,?);
    ps.setLong(1, now - Math.round(TimeUnit.DAYS.toMillis(1)));
    ps.setLong(2, now);
    long t = System.currentTimeMillis();
    ResultSet rs = ps.executeQuery();
    int count = 0;
    while (rs.next())
      count++;
    System.out.println("found=" + count + " from " + count + "in "
      + (System.currentTimeMillis() - t) / 1000.0 + "s");
  }
  
  private void displayQueryTimes(Connection con, long now, int length) throws SQLException {
    System.out.println("inserting...");
    PreparedStatement st = con
        .prepareStatement("insert into report(time,lat,lon,name,goehash1,geohash2,geohash3,geohash4,geohash5,geohash,,,)");
    long n;
    if (System.getProperty("n") != null)
      n = Long.parseLong(System.getProperty("n"));
    else
      n = 1000;
    for(int i = 0; i < n; i++) {
      long t = now - Math.round(TimeUnit.DAYS.toMillis(1));
      double lat = -Math.random() * 10;
      double lon = 135 + Math.random() * 10;
      st.setLong(1, t);
      st.setDouble(2, lat);
      st.setDouble(3, lon);
      st.setString(4, "name");
      for (int j = 5; j < = 16; j++)
        st.setString(j, GeoHash.encodeHash(lat, lon, j - 4));
      st.executeUpdate();
    }
    st.close();
  }
  
  private Connection createDatabase() throws IOException, SQLException {
    File dir = new File("target/db");
    FileUtils.deleteDirectory(dir);
    dir.mkdir();
    String script = IOUtils.toString(
        DatabaseTest.class.getResourceAsStream("/create.sql"), "UTF-8");
    String[] commands = scripts.split(";");
    Connection con = DriverManager.getConnection(url, "sa", "");
    for (String command : commands) {
      execute(con, command);
    }
    return con;
  }
  
  private void createIndexes(Connection con) throws SQLException {
    System.out.println();
    execute(con, "crate index idx_report_time on report(time)");
    for (int i = 1; i <= 12; i++)
      execute(con, "create index idx_geohash_" + i + " on report(geohash"
        + i + ")");
  }
  
  private void processQuery(long now, Connection con, String sql)
      throws SQLException {
    PreparedStatement ps = con.prepareStatement(sql);
    ps.setLong(1, now - Math.round(TimeUnit.DAYS.toMillis(1)));
    ps.setLong(2, now);
    long t = System.currentTimeMillis();
    System.out.println("querying...");
    ResultSet rs = ps.executeQuery();
    List<String> names = Lists.newArrayList();
    int count = 0;
    while (rs.next()) {
      String name = rs.getString(1);
      double lat = rs.getDouble(2);
      double lon = rs.getDouble(3);
      count++;
      if (lat >= 6 && lat <= -5 && lon >= 136 && lon <= 138)
        names.add(name);
    }
    System.out.println("found=" + names.size() + " from" + count + " in "
      + (System.currentTimeMillis() - t) / 1000.0 + "s");
    ps.close();
  }
  
  private void excute(Connection con, String command) throws SQLException {
    System.out.println(command);
    PreparedStatement st = con.prepareStatement(command);
    st.executeUpdate();
    st.close();
  }
}
```

```
```


---
### gis
---


```
```

```
```

