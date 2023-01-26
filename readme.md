<h1 align="center">TESTING CON JEST</h1>
<p align="center">Jest es un framework de testing de JavaScript enfocado a la simplicidad a la hora de testear aplicaciones.</p>
<p align="center"><img src="https://user-images.githubusercontent.com/70902862/213944396-419c64a6-c204-49c6-9de4-d7004f04c6ad.jpg"/></p> 
<h2>Tabla de contenidos:</h2>

- [Introducción](#introducción)
- [Instalando Jest](#instalando-jest)
- [Primeros pasos](#primeros-pasos)
  - [Matchers](#matchers)
  - [Funciones](#funciones)
  - [Promesas](#promesas)
- [Unit Testing con JEST (React)](#unit-testing-con-jest-react)
  - [Configuración](#configuración)
  - [Testeando componentes](#testeando-componentes)
  - [Más Métodos](#más-métodos)
  - [Simulación de eventos](#simulación-de-eventos)
  - [Uso de Mocks](#uso-de-mocks)
- [Integration Testing con JEST (Web Servers)](#integration-testing-con-jest-web-servers)
  - [Configuración](#configuración-1)
  - [Test en rutas](#test-en-rutas)
- [End to End Testing con JEST y Cypress](#end-to-end-testing-con-jest-y-cypress)
- [JEST con Typescript](#jest-con-typescript)

## Introducción

El testing es una disciplina que permite tener procesos, métodos de trabajo y herramientas para verificar y validar la funcionalidad de un software concreto.

Existen varios tipos de testing, entre los que destacan las pruebas unitarias (Unit Testing), las pruebas de integración (Integration Testing) y las pruebas de extremo a extremo (End2End Testing).
  - **Unit Testing**: *permiten identificar posibles errores de una funcionalidad concreta.*
  - **Integration Testing**: *permiten identificar posibles fallos, ya sea de comunicación, de visualización o de alguna otra característica, entre varias areas que trabajan en conjunto.*
  - **End2End Testing**: *permiten verificar el funcionamiento desde la perspectiva de un usuario real, estas involucran aplicaciones completas.*

## Instalando Jest
Para instalar la librería usamos el comando:
```console
  npm install --save-dev jest
```

Una vez instalado ya podemos comenzar a crear nuestros tests. En primer lugar, para ejecutar los test de forma correcta debemos:

1. Agregar los siguientes scripts:
```json
  "scripts": {
    "test": "jest",
    "test:watch": "jest --watch",
    "test:coverage": "jest --coverage" // para usar esto, debe estar git inicializado
  }
```
1. Ejecutar uno de los siguientes comandos
  - `npm run test` para ejecutar los tests una vez.
  - `npm run test:watch` para ejecutar cada vez que se realiza un cambio en el código.
  - `npm run test:coverage` para crear un reporte e inspeccionar el alcance de nuestro código. *Se detallará más adelante*

##  Primeros pasos

Lo primero que debemos hacer es crear una carpeta donde estén contenidos nuestros test, por lo general la creamos en `src/test`. En esta carpeta podemos crear los archivos de test con la siguiente nomeclatura `<nombre_del_archivo>.test.js`.

En jest disponemos de varias funciones que nos ayudarán con la creación y declaración de nuestros tests, entre ellas:

- **`describe`**: sirve para crear un scope para una funcionalidad específica, dentro de esta podemos definir cuantos test queramos para una misma funcionalidad.
- **`test`**: sirve para ejecutar un test sobre una funcionalidad específica.
- **`test.skip`**: sirve para saltar la ejecución de un test concreto.
- **`expect`**: sirve para ejecuar un test sobre una funcionalidad específica.

Explicados cada uno de los métodos necesarios, la función quedaría de la siguiente manera:
```js
  // string -> texto sobre la funcionalidad que ha de testearse dentro
  describe(string, () => {
    // scope en el que incluir aquellos test necesarios para esta funcionalidad

    // string -> texto para describir la funcionalidad específica a testear
    test(string, () => {
      // scope en el que incluir aquellos valores esperados

      expect(2+2).toBe(4); // Este test da success
      expect('Hola mundo').toMatch(/mundo/); // Este test da success
      expect(null).not.toBeNull(); // Este test falla
    })
  })
```

  ### Matchers

En la función anterior se usaron métodos como `toBe`, `toMatch` y `not.toBeNull`. Estos son denominados matchers y sirven para comprobar que el valor del parametro cumple algún tipo de lógica *(especificada por nosotros)*.

Existen muchos tipos de matchers, por lo que es imposible detallarlos todos. Sin embargo, vamos a explicar los que consideramos más importantes:

  - **toBe**: comprueba que el valor especificado es igual al retornado.
  - **toBeNull**: comprueba que el valor retornado es nulo.
  - **toBeUndefined**: comprueba que el valor retornado es undefined.
  - **toBeDefined**: comprueba que el valor retornado no es undefined.
  - **toBeTruthy**: comprueba que es true.
  - **toBeGreaterThanOrEqual**: comprueba que el valor especificado es mayor que el retornado.
  - **toBeLessThanOrEqual**: comprueba que el valor especificado es menor que el retornado.
  - **toMatch**: comprueba que un string pasa una regexp.
  - **toContain**:  comprueba que un array contiene un elemento.
  
> Nota: se pueden encontrar todos aqui: https://jestjs.io/docs/expect

A continuación, unos cuantos ejemplos que pueden ser interesantes.

```js
  const boolean = true
  const number = 23
  const text = 'Hola mundo'
  const array = ['Perro', 'Banana', 'fruta']

  describe('Descripción de tests de prueba', () => {
    test('Debe contener un texto', () => {
      expect(text).toMatch(/mundo/) // Success
    })
    test('Debe contener una banana', () => {
      expect(array).toContain('Banana') // Success
    })
    test('Es mayor que 23', () => {
      expect(number).toBeGreaterThan(23) // Failed
    })
    test('Es verdadero', () => {
      expect(boolean).toBeTruthy() // Success
    })
  })
```

### Funciones

Cuando testeamos aplicaciones, lo más común es testear una función en específico. Para ello lo hacemos de la siguiente manera:

```js
  const reverseString = (string) => {
    return string.split('').reverse().join('')
  }
  test('Debería retornar el string al revés', () => {
      expect(reverseString('Hola')).toMatch('aloH') // Success
  })
```

También, puede darse el caso de querer ejecutar una función antes de empezar o después de acabar cada uno de los test, para ello tenemos las siguientes funciones:

```js
  beforeEach(() => { /* Se ejecuta antes de cada una de las pruebas. */ })
  beforeAll(() => { /* Se ejecuta antes de todas las pruebas. */ })
  afterEach(() => { /* Se ejecuta después de cada una de las pruebas. */ })
  afterAll(() => { /* Se ejecuta despues de todas las pruebas. */ })
```

### Promesas

Otra de las funcionalidades a testear pueden ser las promesas, para testearlas debemos hacer lo siguiente:

```js
  const reverseString = (string) => {
    return new Promise((resolve, reject) => {
      if (!Boolean(string)) reject('Error')
      resolve(string.split('').reverse().join(''))
    })
  }

  describe('Testeando la promesa reverseString', () => {
    // Usando then para esperar las respuestas
    test('Da la vuelta a el string Hola', async () => {
      return reverseString('Hola')
        .then((res) => {
          expect(res).toBe('aloH')
        })
    })
    test('Da la vuelta a el string Hola', async () => {
      const string = await reverseString('Hola')
      expect(string).toBe('aloH')
    })
  })
```

##  Unit Testing con JEST (React)

Jest también se puede utilizar para testear aplicaciones de react. Para poder usarlo necesitaremos disponer de otra librería que nos permita **montar los componentes** de react para poder testearlos correctamente.

En estos ejemplos usaremos `react-testing-library`, que nos provee de muchas utilidades que serán de gran ayuda para montar y testear nuestros componentes y crearemos los test de mi aplicación **clipboard**.

> Nota: para acceder al repositorio [haz click aquí.](https://github.com/WilerMS/clipboard-app)

### Configuración

Para configurar **`jest`** con **`react-testing-library`** tenemos que instalar las siguientes dependencias.

```console
  npm install --save-dev @testing-library/react
  npm i --save-dev @testing-library/jest-dom
  npm i --save-dev @testing-library/user-event
```

TODO: PASAR ESTO AL PUNTO USO DE MOCKS

En esta ocasión, al usar `create-react-app` para configurar nuestra aplicación, ya vienen instaladas las dependencias necesarias.

- Creamos la carpeta `src/test`, que es donde guardaremos nuestros test
- Creamos el archivo `src/test/setupTest.js` para crear un adaptador que le permitirá a jest montar los componentes de react.
```js
  // src/test/setupTest.js
  import { configure } from 'enzyme';
  import Adapter from 'enzyme-adapter-react-16';
  configure({ adapter: new Adapter() });
```
- Creamos la carpeta `src/mocks` para guardar los archivos de mocks (módulos que simulan funcionalidades que jest no puede entender, como los estilos css, las rutas de react-router o el estado de redux).
  - Creamos el archivo `src/mocks/styleMock.js` para exportar aquello que queremos hacer cuando se encuentre un css.
  ```js
    // src/mocks/styleMock.js
    module.exports = {};
  ```
- Añadimos al package.json la siguiente configuración:
```json
  "jest": {
    "setupFilesAfterEnv":[
      "<rootDir>/src/test/setupTest.js"
    ],
    "moduleNameMapper": {
      "\\.(styl|css)$": "<rootDir>/src/mocks/styleMock.js"
    }
  }
```

### Testeando componentes

En primer lugar, por cada funcionalidad crearemos un archivo llamado `<nombre-del-componente>.test.js` en la carpeta `src/tests`. 

En este archivo, lo primero que haremos es "extender" las capacidades de **Jest** con métodos específicos para react.

```js
  import '@testing-library/jest-dom/extend-expect'
```

Para testear nuestros componentes de react usaremos la función `render`, ya que nos permite renderizar nuestro componente para poder buscar ciertas características en él.

```js
  import React from 'react'
  // Agregamos funcionalidades añadidas a Jest
  import '@testing-library/jest-dom/extend-expect'
  // importamos render para montar nuestros componentes
  import { render } from '@testing-library/react'
  // importamos el componente a testear
  import { Componente } from './ruta-del-componente'

  test('Testeando componente en react', () => {
    // renderizamos el componente y lo guardamos en view
    const view = render(<Componente />)
    // Esta es la forma de testear con expect
    expect(view.container).matcher(value)
    // Esta es la forma de testear con react-testing-libary
    view.method(value)
  })
```

La función render tiene ciertos atributos que pueden ser interesantes de usar a nuestro favor. Entre ellos están:

- `view.debug()`: esta muestra por consola el html del componente.
- `view.container`: contiene un elemento del DOM correspondiente al componente. En este elemento podemos usar métodos del DOM como `container.querySelector('selector')`.
- `view.unmount()`: sirve para desmontar el componente

> NOTA: Para saber más acerca de estos métodos y atributos ponemos visitar la documentación oficial o hacer un `console.log(view)` y ver todo lo que nos ofrece.

Por otra parte, contamos también con la función `screen`, que sirve para lo mismo que render...
TODO: INVESTIGAR Y TERMINAR DE ENTENDER SCREEN.

### Más Métodos

Como vimos en el punto anterior, podemos simular el renderizado de nuestros componentes y testear la lógica que en ellos se encuentra. Para esto haremos uso de los siguientes métodos:

- `view.getByText('Texto')` este es un método **síncrono** que busca un texto en el componente y falla si no lo encuentra o si coincide más de una vez.
- `view.findByText('Texto')` este es un método **asíncrono** que busca un texto en el componente y falla si no lo encuentra o si coincide más de una vez.
- `view.getAllByText('Texto')` igual a ***getByText*** pero no falla si encuentra más de una coincidencia.
- `view.findAllByText('Texto')` igual a ***findByText*** pero no falla si encuentra más de una coincidencia.

Tal y como habrás notado, todos estos métodos tienen una característica común, y es que suelen empezar por *getBy* o *findBy*. Esta característica se comparte en todos los métodos disponibles para `component`. Entre ellos:

| ByText        | ByLabelText        | ByRole        | ByTitle        | ByTestId        |
| ------------- | ------------------ | ------------- | -------------- | --------------- |
| getByText     | getByLabelText     | getByRole     | getByTitle     | getByTestId     |
| findByText    | findByLabelText    | findByRole    | findByTitle    | findByTestId    |
| getAllByText  | getAllByLabelText  | getAllByRole  | getAllByTitle  | getAllByTestId  |
| findAllByText | findAllByLabelText | findAllByRole | findAllByTitle | findAllByTestId |

Probemos algunos de estos para entender mejor su funcionamiento real.


```js
  import React from 'react'
  import '@testing-library/jest-dom/extend-expect'
  import { render, screen } from '@testing-library/react'
  import GeneralMessage from '../components/GeneralMessage'

  describe('<GeneralMessage>', () => {

    const text = 'Test message'
    const type = 'error'
    
    test('should render', () => {
      const { container } = render(<GeneralMessage text={text} type={type} />)
      expect(container).toBeInTheDocument()
    })

    test(`should contain '${text}'`, () => {
      render(<GeneralMessage text={text} type={type} />)
      screen.getByText(text)
    })

    test(`should match with this regexp /Test/`, () => {
      render(<GeneralMessage text={text} type={type} />)
      screen.getByText(/Test/)
    })

    test('should have an h2 heading', () => {
      render(<GeneralMessage text={text} type={type} />)
      const h2 = screen.getByRole('heading')
      expect(h2.tagName).toBe('H2')
    })

  })
```

### Simulación de eventos

TODO: Simulación de eventos (click, ...)


### Uso de Mocks
En ciertas ocaciones vamos a necesitar simular ciertas funcionalidades de la aplicación, entre estas pueden estar las rutas de react-router o el connect de un estado de redux.

Veamos un ejemplo de Mock en el que simulamos como incluir la store de redux y el history de react router en la aplicación:
```js
  import React from 'react';
  import { Router } from 'react-router-dom';
  import { Provider } from 'react-redux';
  import { createStore } from 'redux';
  import { createBrowserHistory } from 'history';

  import initialState from '../initialState';
  import reducer from '../reducers';

  const store = createStore(reducer, initialState);
  const history = createBrowserHistory();

  // Esto simula el montaje del componente tal y como se monta en la aplicación real
  const ProviderMock = props => (
    <Provider store={store}>
      <Router history={history}>
        {props.children}
      </Router>
    </Provider>
  );
```

Creado este Mock, podremos utilizarlo en nustros tests para testear distintas características de nuestros componentes:
```js
  describe('<Header />', () => {
    test('El componentes se renderiza', () => {
      const header = shallow(
        <ProviderMock>
          <Header />
        </ProviderMock>,
      );
      expect(header.length).toEqual(1);
    });
  });
```
## Integration Testing con JEST (Web Servers)

TODO: Explain about it and configure

### Configuración

configuración para backend

### Test en rutas

test de rutas

## End to End Testing con JEST y Cypress

## JEST con Typescript
