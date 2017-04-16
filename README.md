# redux-form-field
Easy way to create fields to form for validations, warnings etc...<br>
based on <a href="http://redux-form.com/6.6.3/" target="_blank">redux-form</a>

## Introduction
A new architectural approach to creating fields components for redux-form.
While you using this library all the logic of redux-form created behind the scenes.

redux-form-field give you a easy way to convert your Container to 'Form Container',
and use 'Field Components' inside it.

`Form Container` - Container that connected to redux-form-field using `connectWithReduxForm`
`Field Component` - Component that connected to redux-form-field using `createField`

## Quick start

install
```
$ npm install --save redux-form-field
```
Convert your component to Field Component with `createField`
```JSX
export default createField(MyFieldComponent, {
    nyField_1: PropTypes.string.isRequired,
    nyField_2: PropTypes.string.isRequired
});
```
Connect your Form Container to `redux-form` with `connectWithReduxForm`
```JSX
export default connectWithReduxForm(MyFormContainer,
    (state) => {
        return {

        }
    },
    {
        myActions
    },
    {
        form : 'MyFormContainerForm',
        fields: ['MyFieldComponent'],
        myValidateFunction
    }
);
```

## Methods

### `createField`
##### Parameters :
* `component` - the component you created
* `propTypes [OPTIONAL]` - propTypes that will be combined to the returned component.

##### Return :
Return `redux-form Field` component that can be use easly inside forms.

<br/><br/>

### `connectWithReduxForm`
##### Parameters :
* `component (container)` - the component you want to connect to `redux-form`.
* `mapStateToProps [OPTIONAL]` - states you want to connect to `redux`.
* `mapDispatchToProps [OPTIONAL]` - actions you want to connect to `redux`.
* `reduxFormConfig` - `redux-form` configuration.

##### Return :
Return your connected component to `redux` and `redux-form`


## Usage

#### `createField` Example Code
This example show you how to create component and convert it to Field that can be easly to use inside forms to validations, warnings etc...
as you can see inside the `props` that send to your component you can find `meta` and `input` data.

1) `meta` - include the error/warning message and if field is touched. You can figure out more props on 
<a href="http://redux-form.com/6.6.3/" target="_blank">redux-form</a> documentation.
2) `input` - here you get all the inputs props. for example : onChange, name etc ..
  This field make your component like a 'real' input, you can now pass any props that input have and it will auto combine it to the 'real' input inside your component.

```JSX
import React, { Component } from 'react';
import PropTypes from 'prop-types';
import { createField } from 'redux-form-field';

const component = ({ meta: { touched, error, warning }, input, type, label }) => {

    return (
        <div className="form-group">
            <label>{label}</label>
            <div>
                <input {...input} placeholder={label} type={type} className="form-control"/>
                {error}
            </div>
        </div>
    );

};

export default createField(component, {
    type: PropTypes.string.isRequired,
    label: PropTypes.string.isRequired
});
```

#### `connectWithReduxForm` Example Code
This example show how to use your `redux-form-field` inside Form Container
and how to connect him to `redux-form`.
Input and Textarea are `redux-form-field`.

```JSX
import React, { Component } from 'react';
import { connectWithReduxForm } from 'redux-form-field';
import { createPost } from '../../actions/posts/actions_posts';

import { Input, Textarea } from '../../components/core';

class PostsNew extends Component {
    onChg () {
        console.log("Hello redux-form-field");
    }

    render() {
        const { handleSubmit, pristine, reset, submitting }  = this.props;

        return (
            <form onSubmit={handleSubmit(this.props.createPost)} >

                <h3>Create A New Post</h3>

                <Input name="title" type="text" label="Title" onChange={this.onChg} />
                <Input name="categories" type="text" label="Categories" />
                <Textarea name="content" label="Content" onChange={this.onChg} />

                <button type="submit" className="btn btn-primary">Submit</button>

            </form>
        );
    }
}

function validate(values) {
    const errors = {};

    if (!values.title) {
        errors.title = 'Enter a username';
    }

    return errors;
}

export default connectWithReduxForm(PostsNew,
    null,
    {
        createPost
    },
    {
        form : 'PostsNewForm',
        fields: ['title', 'categories', 'content'],
        validate
    }
);
```
