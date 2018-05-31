Create models from either the app builder or in code and metatonic will automatically render your forms for you.

Capable of handling complex business models including forms with tables, dropdowns referencing other models, validations: both built in and custom, quantities with units, and custom editors. 

Metatonic is still under development and so there will be bugs and features that don't fully work yet but the app builder and frontend-only example app should be working if you want to check them out.


```` TypeScript
// One time setup
let metatonicConfig = {
    dataStore: new RestDataStorage('api')),
    componentRegistry: defaultComponentRegistry
};
let app = createMetatonicReduxThunkApp(metatonicConfig);
let context = app.contexts['default'];
let store = createStore(combineReducers({
    metatonic: (s:any,a:any) => context.metatonicReducer(s as any, a as any)
}), applyMiddleware(app.reduxMiddleware));
app.appStore = store;

// Create a form
let [MyForm, formId] = createAndLoadReactReduxFormForRecord(store, context, 'House', '')

// Use the form in app render
class App extends React.Component {
  render() {
    return (
      <div className="App">
          <Provider store={store}>
              <MyForm />
          </Provider>
      </div>
    );
  }
}

````
