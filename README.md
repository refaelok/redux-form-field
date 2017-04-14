# redux-form-field
Easy way to create fields to form for validations, warnings etc...

### Methods

#### `createField`
    ##### Parameters :
    `component` - the component you created
    `propTypes [OPTIONAL]` - propTypes that will be combined to the returned component.

    ##### Return :
    Return `redux-form Field` component that can be use easly inside forms.

#### `connectWithReduxForm`
    ##### Parameters :

    1. `component (container)` - the component you want to connect to `redux-form`.
    2. `mapStateToProps [OPTIONAL]` - states you want to connect to `redux`.
    3. `mapDispatchToProps [OPTIONAL]` - actions you want to connect to `redux`.
    4. `reduxFormConfig` - `redux-form` configuration.

    ##### Return :
    Return your connected component to `redux` and `redux-form`


### Usage

#### `createField` Example Code
    This example show you how to create component and convert it to Field that can be easly to use inside forms to validations, warnings etc...
    as you can see inside the `props` that send to your component you can find `meta` and `input` data.

    1. `meta` - include the error/warning message and if field is touched. You can figure out more props on `<a href="http://redux-   form.com/6.6.3/" target="_blank">redux-form</a>` documentation.
    2. `input` - here you get all the inputs props. for example : onChange, name etc ..
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

#### `connectWithReduxForm` Example Code` documentation

    ```JSX
    import React, { Component } from 'react';
    import { connectWithReduxForm } from 'redux-form-field';
    import { createPost } from '../../actions/posts/actions_posts';

    import { Input, Textarea } from '../../components/core';

    class PostsNew extends Component {
        onChg () {
            console.log("aaaa");
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
