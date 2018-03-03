# Redux



view/app -> action -> reducer

Le reducer récupère l'action et si il y a une action qui demande de changer le nom, le reducer aura une méthode qui dira quoi faire si l'action changer de nom est appelé.

Il récupère l'ancien state, le manipule et éxécute l'axtion demandé et renvoi un nouveau state.

On crée donc un nouveau state et ile le renvoi

Le nouveau state est stocké dans le store (un store pour toute l'application ) qui stock tous les states et le store renvoi a tous les view/app (components) interessé par ce new state

Le reducer récupère l'action et change le state





1- Créer un store

```react
import {createStore} from "redux";
let store = createStore(reducer, 1);
```

2- Créer le reducer qui va recup les actions exécuter et crée un nouveau state suivant l'action

```react
const reducer = (state, action) => {
  switch (action.type) {
    // Action 1 : ADD
    case "ADD": 
      break;
    // Action 2 : SUBSTRACT
    case "SUBSTRACT":
      break;
  }
  // Nouveau state pour le store
  return state;
}
```

3- on dispatch les actions dans le store pour lui indiquer qu'il ya une nouvelle action

```react
store.dispatch({
  // Type de l'action que l'on souhaite dispatcher
  type: "ADD",
  // Valeur correspondant à l'action 
  payload: 10
})
```

4 -

```react
const reducer = (state, action) => {
  switch (action.type) {
    // Action 1 : ADD
    case "ADD": 
      // On récupère l'ancien state et on ajoute 10 ()
      state = state + action.payload
      break;
    // Action 2 : SUBSTRACT
    case "SUBSTRACT":
      break;
  }
  // Nouveau state pour le store
  return state;
}
```

 77Le provider permet de partager le store à tous les components

```react
ReactDOM.render(
    <Provider store={store}>
        <Plateau/>
    </Provider>,
    root
);

```

On définit les propriété auquel auront accée les components de notre application

```react
function mapStateToProps(storeState, props) {
    return {
    // Les user auront accées au contenu d"finit dans le userReducer
      user: state.userReducer
    };
};

export default connect(
    mapStateToProps
)(Plateau)
```

et donne accées à de nouvelle propriété

```react
class App



render() {
	<div className="container">
	<User username={this.props.user.name}
}



```

