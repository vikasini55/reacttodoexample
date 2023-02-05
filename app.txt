import { useState } from 'react';
import { useDispatch, useSelector } from 'react-redux';
import { AddTodoAction, RemoveTodoAction, UpdateTodoAction } from './action/TodoAction';
import './App.css';

function App() {
  //local state of the component
  const [todo, setTodo] = useState({ id: '', userName: '' });
  const [visible, setVisible] = useState(true);
  const [selectedTodo, setSelectedTodo] = useState('');
  const [defaultTodo, setDefaultTodo] = useState('');
  const [editTextName, setEditTextName] = useState('Edit');

  // to action to store/to call your action 
  const dispatch = useDispatch();
  // to access our state we useSelector
  const Todo = useSelector((state) => state.Todo);
  //
  const { todos } = Todo;

  function getCurrentUnixTimestamp() {
    return Math.floor(Date.now() / 1000);
  }

  const handleSubmit = (e) => {
    e.preventDefault();
    console.log("new todo adding ");
    console.log(todo);
    setDefaultTodo('')
    dispatch(AddTodoAction(todo)); 
  };

  const handleUpdate = (e) => {
    console.log("updating todo step1");
    console.log(todo);
    dispatch(UpdateTodoAction(todo));
  };

  const removeHandler = (t) => {
    console.log(t)
    dispatch(RemoveTodoAction(t));
  }

  const removeElement = () => {
    setVisible((prev) => !prev);
  };


  return (
    <div className="App">
      <header className="App-header">
        <h2>Todo List APP</h2>
        <form style={{
          width: 500,
          alignItems: 'center',
          marginLeft: 70
        }} onSubmit={handleSubmit}>
          <input placeholder="Enter a todo notes" style={{
            width: 300,
            padding: 10,
            borderRadius: 5,
            border: "none",
            fontSize: 20,
          }}
            defaultValue={defaultTodo}
            //changing the input box
            onChange={(e) => {
              console.log(e.target.value)
              setTodo({ id:getCurrentUnixTimestamp(), userName: e.target.value })
            }
            }
          />
          <button type='submit' style={{
            width: 150,
            padding: 12,
            color: "Red",
            marginLeft: 20,
            padding: 10,
            borderRadius: 5,
            border: "none",
            fontSize: 20,
          }}>Add</button>
        </form>

        <ul className='todo-block-style'>
          {
            todos && todos.map(y => {
              console.log("inside Map")
              console.log(y)
              return (
                <li key={y.id} className='ul-style'>

                  {
                    selectedTodo !== y.id && (
                      <span className='span-style' key={y.id} >{y.userName}</span>
                    )
                  }

                  {visible && y.id === selectedTodo && (
                    <span className='span-style' key={y.id}>{y.userName}</span>
                  )}

                  {!visible && y.id === selectedTodo &&  (
                    <input className='span-style' type="text" onChange={(e) => setTodo({ id: y.id, userName: e.target.value })} defaultValue={y.userName} />
                  )}

                  <button style={{
                    padding: 12,
                    borderRadius: 10,
                    fontSize: 15,
                    marginLeft: 20,
                  }}
                    onClick={() => {
                      if (visible) {
                        console.log("selected record to update is")
                        console.log(y.id)
                        setSelectedTodo(y.id)
                        setEditTextName("Update")
                        return removeElement()
                      }
                      setEditTextName("Edit")
                      removeElement()
                      return handleUpdate()
                    }}
                  >
                    {y.id==selectedTodo && (editTextName)}
                    {y.id!==selectedTodo && ("Edit")}
                    </button>
                  <button style={{
                    padding: 12,
                    borderRadius: 10,
                    fontSize: 15,
                    marginLeft: 20,
                    background: 'red',
                    color: 'white'
                  }}
                    onClick={() => {
                      console.log("on delete click")
                      removeHandler(y)
                    }}

                  >Delete</button>


                </li>
              )
            }
            )}
        </ul>
      </header>
    </div>
  )
}

export default App;