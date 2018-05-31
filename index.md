Create models from either the app builder or in code and metatonic will automatically render your forms for you.

Capable of handling complex business models including forms with tables, dropdowns referencing other models, validations: both built in and custom, quantities with units, and custom editors. 

Right now it supports React and Redux but Vue, Angular, and MobX support are all planned.

Metatonic is still under development and so there will be bugs and features that don't fully work yet but the [app builder](/app-builder) and [frontend-only example app](https://github.com/beattyml1/metatonic/tree/master/apps/frontend-only) should be mostly working if you want to check them out.

### Install
````sh
npm install metatonic-core metatonic-redux metatonic-react metatonic-react-redux
````

### Setup
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

### Models
````TypeScript
@model()
export class House {
    @field("Currency", "Asking Price", {min: '0', max: '100000000'})
    askingPrice: Currency;

    @field("Address", "Address", { canAdd: true })
    address: Address;

    @list("Person", "Homeowners", { canAdd: true, uiControlPreference: 'repeater' })
    homeowners: Homeowner[];
}

@model()
export class Person {
    @field("text", "First Name")
    firstName: string;

    @field("text", "Last Name")
    lastName: string;

    @field("Address", "Address", { canAdd: true })
    address: Address
}

@model('Address')
export class Address {
    @field("text", "Address Line 1")
    address1: string;

    @field("text", "Address Line 2")
    address2: string;

    @field("text", "City")
    city: string;

    @select("State", "State")
    state: string;

    @field("text", "Zip Code", { maxLength: 5 } )
    zipCode: string;
}
````
