# Implement Error Boundary on React Project

## 1) Reference
https://legacy.reactjs.org/docs/error-boundaries.html

## 2) Note
```bash
Error boundaries do not catch errors for:
- Event handlers (learn more)
- Asynchronous code (e.g. setTimeout or requestAnimationFrame callbacks)
- Server side rendering
- Errors thrown in the error boundary itself (rather than its children)
```

## 3) Error Boundary Architecture
![image](https://github.com/denitiawan/research-react-error-boundary/assets/11941308/0889962b-58ef-4a8f-b46c-b43049709ed7)

## 4) Screenshoot Fallback-UI from Error Boundary
![image](https://github.com/denitiawan/research-react-error-boundary/assets/11941308/354ab832-28be-44c4-b975-793fc3112cef)
![image](https://github.com/denitiawan/research-react-error-boundary/assets/11941308/5d459daa-160b-482c-8d82-68145add00f6)




## 5) Examples
- [ErrorBoundary.js](#errorboundaryjs)
- [App.js](#appjs)
- [Home.js](#homejs)
  
### ErrorBoundary.js
```bash

import React from 'react';
import { Box, Container, CssBaseline, TextareaAutosize, Typography } from "@mui/material"

export class ErrorBoundary extends React.Component {

    constructor(props) {
        super(props);
        this.state = {
            hasError: false,

            error: {}, 
            errorDetails: undefined 
        };
    }

    // Show the Fallback UI
    static getDerivedStateFromError(error) {
        return { hasError: true };
    }

    // Catch the error
    componentDidCatch(error, errorInfo) {        
        this.setState({
            error: error,
            errorDetails: errorInfo.componentStack
        });
      
      // todo here : your custom error handling
    }

    function doReloadPage() {
        // todo here : your custom error handling
    
        // reload page
        window.location.reload();
    }

    render() {        
        const { hasError, errorDetails } = this.state;


        if (hasError) {

           // Custom Fallback UI
            return <div>

                <Container component="main" maxWidth="xs">
                    <CssBaseline />
                    <Box
                        sx={{
                            marginTop: 8,
                            display: 'flex',
                            flexDirection: 'column',
                            alignItems: 'center',
                        }}
                    >

                        <h1>Internal Server Error</h1>

                        <Typography component="h1" variant="h5">
                            Opps... Something went wrong...!
                        </Typography>

                        <Box noValidate sx={{ mt: 1 }}>
                            
                            <TextareaAutosize
                                maxRows={10}
                                aria-label="maximum height"
                                placeholder="Maximum 4 rows"
                                defaultValue={errorDetails}
                                style={{ width: 1000 }}
                            />
                        </Box>

                        <Button
                            type="submit"
                            variant="contained"
                            sx={{ mt: 1, mb: 2, textAlign: 'right' }}
                            onClick={() => doReloadPage()}>
                                Reload Page
                        </Button>

                    </Box>
                </Container>
            </div>

        }

        return this.props.children;
    }
}

```

### App.js
```react
import {Route, Routes} from "react-router-dom"; // navigate page
import {ErrorBoundary} from "./error/ErrorBoundary";
function App() {
    return (
        <ErrorBoundary>
            <Routes>
                <Route path="/" element={<Home />}/>
            </Routes>
        </ErrorBoundary>
    );
}
export default App;


```
### Home.js
```bash
import {Box} from "@mui/material";
const Dashboard = ({baseImplements}) => {

   useEffect(() => {        
        // example for thrown error when fetch data
        throw new Error('Oooops, We are crash!');
    }, []);

return (
        <Box> Welcome Home </Box>
    );
};

export default Dashboard;

```




