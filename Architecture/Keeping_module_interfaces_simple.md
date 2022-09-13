# Keeping module interfaces simple

Lets consider we have an editable table React module with all operations exposed. It also is a 
connected component that gets its data from Redux datastore.

```jsx

interface Props {
  database: string,
  tableId: string,
  
  onAddRowBefore: (rowId: string) => {}
  onAddRowAfter: (rowId: string) => {}
  onDeleteRow: (rowId: string) => {}
  
  onAddColumnBefore: (rowId: string, columnId: string) => {}
  onAddColumnAfter: (rowId: string, columnId: string) => {}
  onDeleteColumn: (rowId: string, columnId: string) => {}

  onChangeCellValue: (rowId: string, columnId: string, value: string) => {}

  onSelect?: (rowId: string, columnId: string) => {}
  onBlur?: (rowId: string, columnId: string) => {}
  onHover?: (rowId: string, columnId: string) => {}
  onMouseLeave?: () => {}
}

class EditableTable extends PureComponent<Props> {
```

One of the problems of the code is that the module is doing multiple things at a time:
  * Connecting to data store and fetching the data
  * rendering the table

This interface is quite big and cluttered. It also hides the knowledge that
the code has to actually fetch the data beforehand and put it somewhere (no reference where).
Binding this component probably needs a lot of digging into documentation or even worse the code.

**By separating the matter of concers so that view does only view things and connector does only
connector things, interface becomes more transparent.**

By grouping callbacks to more generic ones with type property, developer can get the component ready to render in no time and basically get the types af feel how things work by just interfacing with components.
Everybody likes an interface you can learn by using better than diving into documentation.

Also as a sideEffect the EditableTable becomes reusable and is not strictly restricted to specific Redux
properties.

```jsx
interface Props {
  headers: ({ columnId:string, title: string })[];
  rows: ({ rowId: string, columnId:string, value: string })[];

  onChange: ({
    type: 'addRowBefore' | 'addRowAfter' | 'deleteRow' |
          'addColumnBefore' | 'addColumnAfter' | 'deleteColumn' |
          'changeValue';
    data: ... 
  }) => (); 

  onEvent?: ({
    type: 'select' | 'blur' | 'hover' | 'leave';
    data: ...
  })
}

class EditableTable extends PureComponent<Props> {
```

```jsx
class DbTable PureComponent<Props> {
  render() {

    const { data, handleDbChange, handleDbEvent } = this.props;

    return(
      <EditableTable
        headers={ data.headers }
        rows={ datta.rows }

        onChange={ handleDbChange }
        onEvent={ handleDbEvent }
      >
  }
}

const mapStateToProps = (state, ownProps) => {
  const {  database, tableId } = ownProps;
  return {
    data: selectRowFromState(state, database, tableId);
  }
};

const mapDispatchToProps = (dispatch, ownProps) => {
  const {  database, tableId } = ownProps;
  return {
    handleDbChange: ({type, data}) => { },
    handleDbEvent: ({type, data}) => { },
  };
);

export default connect(mapStateToProps)(Row);

```

Interfaces become simpler if you think about what belongs together, not what is the task at hand.