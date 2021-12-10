# config-ambienteReact
Minha configuração de ambiente em react

- *Criando estrutura do projeto*
    - Criar pasta do projeto - ***mkdir***
    - Criar o package.json -  ***npm or yarn init -y***
    - Instalar dependências - **biblioteca**
    - Instalar biblioteca (React) - ***yarn add react***
    - Instalar o React Dom (Html convertido em um objeto **JS**) - ***yarn add react-dom***
    - Criar estrutura de pastas
        - src = para components, index.js
        - public = para html, fiaticon, robot.txt
- *Configurando Babel*
    
    O babel ele converte o Javascript em Html para que o navegador entenda - babel.js.io
    
    - yarn add @babel/core @babel/cli @babel/preset-env -D
    - Criar um arquivo babel.config.js
    - **babel.config.js**
        
        ```tsx
        module.exports = {
            presets: [
              '@babel/preset-env',
              '@babel/preset-typescript',
              ['@babel/preset-react', {
                runtime: 'automatic'
              }]
            ]
          }
        ```
        
    - yarn babel src/index.js —out-file dist/bundle.js
    - yarn add @babel/preset-react -D
- *Configurando Webpack*
    
    yarn add webpack webpack-cli webpack-dev-server -D
    
    yarn webpack
    
    yarn add html-webpack-plugin -D
    
    yarn add cross-env -D
    
    yarn add style-loader css-loader -D
    
    yarn add sass-loader -D
    
    yarn add node-sass -D
    
    - Criar arquivo webpack.config.js
    - **webpack.config.js**
        
        ```tsx
        const path = require('path')
        const HtmlWebpackplugin = require('html-webpack-plugin')
        const ReactRefrashWebpack = require('@pmmmwh/react-refresh-webpack-plugin')
        
        const isDevelopment = process.env.NODE_ENV !== 'production'
        
        module.exports = {
          mode: isDevelopment ? 'development': 'production',
          performance: {
            hints: false,
            maxEntrypointSize: 512000,
            maxAssetSize: 512000
          },
          
          devtool: isDevelopment ? 'eval-source-map' : 'source-map',
        
          entry: path.resolve(__dirname, 'src', 'index.tsx'),
          output: {
            path: path.resolve(__dirname, 'dist'),
            filename: 'bundle.js'
          },
          resolve: {
            extensions: ['.js', '.jsx', '.ts', '.tsx']
          },
          devServer: {
            static:{
              directory : path.join(__dirname, 'public'),
            },
            hot: true
          },
        
          plugins: [
            isDevelopment && new ReactRefrashWebpack,
            new HtmlWebpackplugin({
              template: path.resolve(__dirname, 'public', 'index.html')
            })
          ].filter(Boolean),
          
          module: {
            rules: [
             {
              test: /\.(j|t)sx$/, 
              exclude: /node_modules/,
              use: {
                loader: 'babel-loader',
                options: {
                  plugins: [
                    isDevelopment && require.resolve('react-refresh/babel')
                  ].filter(Boolean)
                }
              }
            },
            
            {
              test: /\.scss$/,
              exclude: /node_modules/,
              use: ['style-loader', 'css-loader', 'sass-loader']
            },
          ]
          }
        
        }
        ```
