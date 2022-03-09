# Front-end App with React and Ant Design Pro

## 1. Project folder

    .vscode
    config
    dist                                # Output files
    mock                                # Mock Server Folder
    node_modules
    public
    src                                 # Source Code Folder
    tests
    .editorconfig
    .eslintignore
    .eslintrc.js
    .gitignore
    .prettierignore
    .prettierrc.js
    .stylelintrc.js
    jest.config.js
    jsconfig.json
    package.json
    playwright.config.js
    README.md
    yarn.lock

## 2. Src folder

    .umi
    components
    e2e
    locales
    pages                           # View layer
    services                        # Network request layer
    access.js
    app.jsx                         # main entry of the application
    global.jsx
    global.less
    manifest.json
    service-worker.js


## 3. Create a new page

1. New folder: /src/pages/products
2. New file: /src/pages/products/index.jsx
3. Add Code:

```javascript
import { PageContainer } from '@ant-design/pro-layout';
import { Card } from 'antd';
import { useIntl } from 'umi';


const Products = () => {
  const intl = useIntl();
  return (
    <PageContainer>
      <Card>
       Products
      </Card>
    </PageContainer>
  );
};

export default Products;

```

### 4. Add code to Menu /config/routes.js (after welcome)

```javascript

  {
    path: '/products',
    name: 'products',
    icon: 'smile',
    component: './products',
  },

```


### 5. You can try adding some new pages and components with ANTD PRO.

- Button
- Typography
- Alert
- message
- Input
- Table
- Drawer

Take a look at Welcome.jsx
