# How-to-create-empty-rows-in-Flutter-DataTable-SfDataGrid-

The Syncfusion Flutter DataGrid requires the collection of DataGridRow. Users typically convert the underlying business object collection to a collection of DataGridRow at the sample level. Users must set the collection to the DataGridSource.rows property. 

You can add the additional data directly to the DataGridSource.rows property. So, additional empty rows without having those rows in underlying business object data collection can also be added.

The following steps explain how to create additional empty rows in Syncfusion Flutter DataTable:

## STEP 1: 
Initialize the SfDataGrid widget with all the required properties. 

```xml
  List<Employee> _employees = <Employee>[];
  late EmployeeDataGridSource _employeeDataSource;

  @override
  void initState() {
    super.initState();
    _employees = getEmployeeData();
    _employeeDataSource =
        EmployeeDataGridSource(employeeData: _employees, emptyRowsCount: 5);
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
        appBar: AppBar(title: const Text('Syncfusion Flutter DataGrid')),
        body: SfDataGrid(
            source: _employeeDataSource,
            columnWidthMode: ColumnWidthMode.fill,
            columns: <GridColumn>[
              GridColumn(
                  columnName: 'id',
                  label: Container(
                      padding: const EdgeInsets.all(16.0),
                      alignment: Alignment.center,
                      child: const Text('ID'))),
              GridColumn(
                  columnName: 'name',
                  label: Container(
                      padding: const EdgeInsets.all(8.0),
                      alignment: Alignment.center,
                      child: const Text('Name'))),
              GridColumn(
                  columnName: 'designation',
                  label: Container(
                      padding: const EdgeInsets.all(8.0),
                      alignment: Alignment.center,
                      child: const Text('Designation',
                          overflow: TextOverflow.ellipsis))),
              GridColumn(
                  columnName: 'salary',
                  label: Container(
                      padding: const EdgeInsets.all(8.0),
                      alignment: Alignment.center,
                      child: const Text('Salary'))),
            ]));
  }
```

## STEP 2: 
Create a data source class by extending DataGridSource for mapping data to the SfDataGrid. Create empty rows by setting the null values to the cells and adding them to the  DataGridRow collection. These empty rows are added after converting the business object collection to DataGridRow collection.

```xml
class EmployeeDataGridSource extends DataGridSource {
  EmployeeDataGridSource(
      {required List<Employee> employeeData, required int emptyRowsCount}) {
    _dataGridRows = employeeData
        .map<DataGridRow>((e) => DataGridRow(cells: [
              DataGridCell<int>(columnName: 'id', value: e.id),
              DataGridCell<String>(columnName: 'name', value: e.name),
              DataGridCell<String>(
                  columnName: 'designation', value: e.designation),
              DataGridCell<int>(columnName: 'salary', value: e.salary),
            ]))
        .toList();
    for (int i = 0; i < emptyRowsCount; i++) {
      _dataGridRows.add(const DataGridRow(cells: [
        DataGridCell(columnName: 'id', value: null),
        DataGridCell(columnName: 'name', value: null),
        DataGridCell(columnName: 'designation', value: null),
        DataGridCell(columnName: 'salary', value: null),
      ]));
    }
  }

  List<DataGridRow> _dataGridRows = [];

  @override
  List<DataGridRow> get rows => _dataGridRows;

  @override
  DataGridRowAdapter buildRow(DataGridRow row) {
    return DataGridRowAdapter(
        cells: row.getCells().map<Widget>((e) {
      return Container(
        alignment: Alignment.center,
        padding: const EdgeInsets.all(8.0),
        child: Text(e.value?.toString() ?? ''),
      );
    }).toList());
  }
}
```