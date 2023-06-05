# Guide to understand web3-react librbrary

The web3-react linraries have following core modules: 

core: This module contains the core functionality of web3-react. It includes the core context and hook implementations, which are the central components for managing the web3 connection and handling Ethereum-related operations.

components: This module provides a set of pre-built React components that can be used to build user interfaces for interacting with Ethereum. These components are designed to work seamlessly with web3-react and provide convenient wrappers for common tasks like connecting to the network, displaying account information, and handling transactions.

hooks: This module contains additional hooks that can be used alongside web3-react. These hooks provide useful abstractions and utilities for working with Ethereum, such as fetching account balances, monitoring blockchain events, and managing transaction state.

types: This module defines TypeScript type definitions for web3-react and its related modules. 

helpers: This module contains various helper functions and utilities that can assist with common tasks when working with web3-react. These helpers simplify tasks like handling Ethereum network IDs, converting between different Ethereum units, and checking the availability of certain features and the state.

test-utils: This module provides testing utilities specifically designed for web3-react. It includes helper functions and mocks that can be used to write unit tests or integration tests for code that relies on web3-react.

## Core 

The `core` module in the web3-react package is responsible for providing the fundamental functionality and infrastructure for managing the web3 connection and Ethereum-related operations in your application. It consists of the following components:

`Web3ReactProvider`: This is the central component of the core module. It serves as a provider for the web3-react context, which allows components in your application to access the web3 connection and interact with Ethereum. The Web3ReactProvider takes in a configuration object that specifies the web3 provider options and creates a context that can be used by other components.

`useWeb3React`: This is a custom React hook provided by the core module. It allows components to access the web3-react context and retrieve the web3 connection, current account, and other relevant information. By using this hook, you can easily integrate web3 functionality into your components and react to changes in the web3 connection or account.

`Web3ReactContext`: This is the context object created by the Web3ReactProvider. It holds the web3 connection, current account, and other relevant data. This context is used by the useWeb3React hook and can be accessed by any component wrapped within the Web3ReactProvider.

`ActivationContex`t: This context is used for tracking the activation state of the web3-react context. It keeps track of whether the web3 connection is actively being used by your application or if it's idle. This information can be useful for handling scenarios like disconnecting from the provider when the application is not actively using it.

`Connector`: The connector is responsible for connecting your application to the Ethereum network using a specific web3 provider. The core module provides a default connector called AbstractConnector, which can be extended to create custom connectors for different providers like MetaMask, WalletConnect, or other Ethereum wallets.

These core components work together to manage the web3 connection and provide an interface for your application to interact with Ethereum. By utilizing the Web3ReactProvider and useWeb3React hook, you can easily integrate web3 functionality into your React components and build applications that interact with the Ethereum blockchain.

### Connectors

The terms "provider" and "connector" have distinct meanings in the context of web3-react:

Provider: In the context of web3-react, a provider is a software component that establishes the connection between your application and the Ethereum network. It acts as an intermediary, allowing your application to interact with the blockchain by sending requests, signing transactions, and retrieving blockchain data. Providers handle the low-level communication with the Ethereum network and provide an interface for your application to interact with it. Examples of providers include the Injected Provider (e.g., MetaMask), JSON-RPC Provider, WebSocket Provider, etc.

Connector: A connector, on the other hand, is a higher-level concept within web3-react. It represents a specific implementation for connecting to a provider. Connectors encapsulate the logic required to establish a connection and handle authentication with a particular web3 provider. Connectors are designed to simplify the integration process by providing a unified interface regardless of the underlying provider. Examples of connectors include Injected Connector (for MetaMask and other injected providers), WalletConnect Connector, Portis Connector, etc.

In summary, a provider is a low-level component that handles the communication with the Ethereum network, while a connector is a higher-level abstraction that simplifies the integration process for connecting to a specific provider. Connectors encapsulate the necessary logic to establish a connection, authenticate, and interact with the underlying provider, while providers handle the actual communication with the Ethereum network.



Setting up web3-react for different connectors involves configuring and integrating the desired web3 providers or wallets into your application. Connectors act as the bridge between your application and the web3 provider, enabling communication with the Ethereum network. Here's how you can set up web3-react with different connectors:

* Choose a Connector: Decide on the connectors you want to support in your application. Some popular connectors include MetaMask, WalletConnect, Portis, Fortmatic, and Torus. Each connector has its own implementation and setup process.

Install Required Packages: Install the necessary packages for the specific connector you want to use. For example, if you're using MetaMask as a connector, you can install the @web3-react/metamask package:

// bash
`npm install @web3-react/metamask`

* Import and Configure the Connector: Import the connector package into your code and configure it within the Web3ReactProvider component. Here's an example for using the MetaMask connector:

// jsx
`import { Web3ReactProvider } from '@web3-react/core';
import { InjectedConnector } from '@web3-react/injected-connector';

// Create an instance of the MetaMask connector
const injectedConnector = new InjectedConnector({ supportedChainIds: [1, 3, 4, 5, 42] });

// Function to create the web3-react context
const getLibrary = (provider) => {
  // Create a new web3 instance using the provider
  const library = new Web3Provider(provider);
  library.pollingInterval = 12000;
  return library;
};

// Wrap your app with the Web3ReactProvider and provide the connector and library configuration
ReactDOM.render(
  <Web3ReactProvider getLibrary={getLibrary}>
    <App />
  </Web3ReactProvider>,
  document.getElementById('root')
);`

In this example, we create an instance of the InjectedConnector (MetaMask) and provide the supported chain IDs. We also define the getLibrary function, which initializes a web3 instance using the provider.

* Use the useWeb3React Hook: Now, you can use the useWeb3React hook within your components to access the web3-react context and interact with the selected connector. For example:

// jsx
`import { useWeb3React } from '@web3-react/core';

function MyComponent() {
  const { account, library } = useWeb3React();

  // Use the account and library variables to interact with Ethereum

  return (
    // JSX for your component
  );
}`
By utilizing the useWeb3React hook, you can access the current account and web3 library instance provided by the selected connector.

* Handle Connector Activation: Connectors usually require user interaction for activation. You can use the activate function from the useWeb3React hook to trigger the activation process. For example:

// jsx
`import { useWeb3React } from '@web3-react/core';

function MyComponent() {
  const { activate } = useWeb3React();

  const handleConnect = () => {
    activate(injectedConnector);
  };

  return (
    <button onClick={handleConnect}>Connect with MetaMask</button>
  );
}`

In this example, clicking the button triggers the handleConnect function, which calls the activate function with the injected connector. This initiates the MetaMask activation process and prompts the user to connect their MetaMask wallet to your application.  
