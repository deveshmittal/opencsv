

# General #

## What is opencsv? ##

opencsv is a very simple csv (comma-separated values) parser library for Java. It was developed because all of current csv parsers I've come across don't have commercial-friendly licenses.

## Where can I get it? ##

Source and binaries are available [here](https://code.google.com/p/opencsv/downloads/list). You can check out the [javadocs](https://opencsv.googlecode.com/hg/doc/api/index.html) online.

## What features does opencsv support? ##

opencsv supports all the basic csv-type things you're likely to want to do:

  * Arbitrary numbers of values per line
  * Ignoring commas in quoted elements
  * Handling quoted entries with embedded carriage returns (ie entries that span multiple lines)
  * Configurable separator and quote characters (or use sensible defaults)
  * Read all the entries at once, or use an Iterator style model
  * Creating csv files from String[.md](.md) (ie. automatic escaping of embedded quote chars)

# Reading and Writing #

## How do I read and parse a CSV file? ##

```
CSV csv = CSV.create();
csv.read("example.csv", new CSVReadProc() {
    public void procRow(int rowIndex, String... values) {
        System.out.println(rowIndex + ": " + Arrays.asList(values));
    }
});
```

## Can I use my own separators and quote characters? ##

Yes. Say you're using a tab for your separator, you can do something like this:

```
CSV csv = CSV
    .separator('\t')
    .create();       
```

And if you single quoted your escaped characters rather than double quote them, you can use the three arg constructor:

```
CSV csv = CSV
    .separator('\t')
    .quote('\'')
    .create();
```

You may also skip the first few lines of the file if you know that the content doesn't start till later in the file. So, for example, you can skip the first two lines by doing:

```
CSV csv = CSV
    .separator('\t')
    .quote('\'')
    .skipLines(2)
    .create();
```

## Can I write csv files with opencsv? ##

Yes. For example:

```

csv.write("yourfile.csv", new CSVWriteProc() {
    public void process(CSVWriter out) {
        out.writeNext("first", "second", "third");
   }
});

```


## Can I dump out SQL tables to CSV? ##

Yes you can. Sean Sullivan added a neat feature to CSVWriter so you can pass writeAll() a ResultSet.

```
java.sql.ResultSet myResultSet = ....
writer.writeAll(myResultSet, includeHeaders);
```

## Is there a way to bind my CSV file to a list of Javabeans? ##

Yes there is. Kyle Miller added a new set of classes to allow you to bind a CSV file to a list of JavaBeans based on column name, column position, or a custom mapping strategy. You can find the new classes in the au.com.bytecode.opencsv.bean package. Here's how you can map to a java bean based on the field positions in your CSV file:

```
ColumnPositionMappingStrategy strat = new ColumnPositionMappingStrategy();
strat.setType(YourOrderBean.class);
String[] columns = new String[] {"name", "orderNumber", "id"}; // the fields to bind do in your JavaBean
strat.setColumnMapping(columns);

CsvToBean csv = new CsvToBean();
List list = csv.parse(strat, yourReader);
```

For more detailed examples, check out the test cases for each of the available mapping strategies under the /test/au/com/bytecode/opencsv/bean/.

# Other Stuff #

## Can I use opencsv in my commercial applications? ##

Yes. opencsv is available under a commercial-friendly Apache 2.0 license. You are free to include it in your commericial applications without any fee or charge, and you are free to modify it to suit your circumstances. To find out more details of the license, read the Apache 2.0 license agreement.

## Can I get the source? More example code? ##

Yes. Please, see instructuins [here](https://code.google.com/p/opencsv/source/checkout). It's just a few files, so go crazy. There is also a sample addressbook csv reader in the /examples directory. And for extra marks, there's a JUnit test suite in the /test directory.

## Who maintains opencsv? ##

opencsv was developed in a couple of hours by Glen Smith. Kyle Miller contributed the bean binding work, Scott Conway has done tons of bug fixing, and Sean Sullivan should be the current maintainer of the project. And no one from these guys do not respond on emails :)