# ToDoList-By-React

基于react的todolist，并通过localStorage保存信息

核心代码

```javascript
if (!localStorage.todolist) {
        localStorage.todolist = JSON.stringify([]);
      }
      var arr = JSON.parse(localStorage.todolist);
      var ToDoList = React.createClass({
        getInitialState: function () {
          return {
            todolist : arr
          };
        },
        handleChange : function (rows) {
          this.setState({
            todolist : rows
          });
          localStorage.todolist = JSON.stringify(arr);
        },
        render:function () {
          return  <div>
                    <header>
                      toDoList
                    </header>

                    <TypeNew onAdd={this.handleChange} todo={this.state.todolist}/>
                    <ListTodo onDel={this.handleChange} todo={this.state.todolist} />
                  </div>
        }
      });
      var TypeNew = React.createClass({
        handleAdd : function (e) {
          e.preventDefault();
          e.stopPropagation();
          var inputDom = this.refs.inputline;
          var newthing = inputDom.value.trim();
          var newdate = this.refs.inputdate.value;
          var rows = this.props.todo;
          if (newthing !== '') {
            rows.push(
            {
              "date" : newdate,
              "thing" : newthing
            }
            );
            this.props.onAdd(rows);
          };
          inputDom.value = '';
        },
        render: function () {
          return (
            <div>
              <input type = "date" ref = "inputdate" className="new-todo"/>
              <input type = "text" ref = "inputline" placeholder="What needs to be done?" className="new-todo"/>
              <button onClick = {this.handleAdd} className="new-todobtn">submit</button>
            </div>
          );
        }
      });
      var ListTodo = React.createClass({
        handleDel : function (e) {
          e.preventDefault();
          e.stopPropagation();
          var rows = this.props.todo;
          rows.splice(e.target.getAttribute('data-key'),1);
          this.props.onDel(rows);
        },
        render : function () {
          var that = this;
          return(   
            <ul>  
              {
                this.props.todo.map(function (item,index,array) {
                  return  <li  key={index}>
                            <span>date : {item.date}</span>
                            <span>  thing : {item.thing}</span>
                            <button onClick={that.handleDel}  data-key = {index} className="new-todobtn">Delete</button>
                          </li>    
                })
              }     
            </ul>
          );
        }
      });
      ReactDOM.render(
        <ToDoList/>,
        document.getElementById('example')
      );
```
