# fetch-redux-middleware

A Redux Middleware that use fetch standard to simplify fetch actions/reducers workflow

##Installation

    npm i fetch-redux-middleware --save

Or use yarn:

    yarn add fetch-redux-middleware

## Configuration

In order to use this middleware, you can follow this example

    import fetchMiddlewareCreator from "redux-fetch-middleware"
    import { applyMiddleware } from 'redux';

    const fetchMiddlewareInstance = fetchMiddlewareCreator({
        base : "https://exampleapi.com,
        defaultHeaders : {
            ["Accept"] : "application/json",
            ["Content-type"] : "application/json"
        },
        defaultParams : {
            api_key : "0123445689"
        },
        onRequest : (request, state, action) => {
            /* this code is called before each request, you can modify it */

            if (state.session.token) {
              request.params["token"] = "jwt.token.1212GJ23"
            }

            return request
        }
    })

    applyMiddleware(fetchMiddlewareInstance);


## How to use it

  Property "param" is optionnal

    function actionCreator(param1)
        return {
            url : "homeData",
            params : {
                dataOrder : params1
            },
            onStart : (payload, meta, dispatch, getState) => {
              /* DO SOMETHING WHEN REQUEST START */
              /*
                # payload : null
                # meta : null
                # dispatch : redux dispatch func
                # getState : redux getState func
              */
            },
            onSuccess : (payload, meta, dispatch, getState) => {
              /* DO SOMETHING WHEN REQUEST SUCCEED */
              /*
                # payload : response body
                # meta : response data (http code status)
                # dispatch : redux dispatch func
                # getState : redux getState func
              */
              /* you can dispatch (and go throught all middlewares chain) */
              dispatch({
                TYPE : "HOMEDATA_SUCCESS"
              })
              /* you can send an action to the next middleware */
              return {
                TYPE : "HOMEDATA_SUCCESS"
              }

            },
            onError : (payload, meta, dispatch, getState) => {
              /* DO SOMETHING WHEN REQUEST FAIL */
              /*
                same possibilies than in onSuccess
              */
            }
        }
    }

## POST Request

  A post request just need a aditionnal property : body. You can use param too in a POST request if its requires query parameters

    function loginAction(email, password)
        return {
            url : "account/auth",
            method: 'POST',
            body : {
                email,
                password
            },
            onStart : (payload, meta, dispatch, getState) => {},
            onSuccess : (payload, meta, dispatch, getState) => {},
            onError : (payload, meta, dispatch, getState) => {}
        }
    }

## Automatically dispatch Actions

  With autoDispatchPrefix : the middleware will dispatch actions automatically. In this case: LOGIN_SUCCESS, LOGIN_REQUEST and LOGIN_ERROR

    function loginAction(email, password)
        return {
            url : "account/auth",
            method: 'POST',
            body : {
                email,
                password
            },
            autoDispatchPrefix: 'LOGIN',
        }
    }
