# useTheme

Version: 7.x

Sitemap: [llms.txt](https://reactnavigation.org/llms.txt)

The `useTheme` hook lets us access the currently active theme. You can use it in your own components to have them respond to changes in the theme.

**Static (Recommended):**

```js name="useTheme hook" snack
import * as React from 'react';
import {
  useNavigation,
  createStaticNavigation,
  DefaultTheme,
  DarkTheme,
} from '@react-navigation/native';
import { View, Text, TouchableOpacity, useColorScheme } from 'react-native';
import { createNativeStackNavigator } from '@react-navigation/native-stack';
import { createDrawerNavigator } from '@react-navigation/drawer';
import { Button } from '@react-navigation/elements';
// codeblock-focus-start
import { useTheme } from '@react-navigation/native';

// codeblock-focus-end

function SettingsScreen({ route }) {
  const navigation = useNavigation();
  const { user } = route.params;
  const { colors } = useTheme();

  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Text style={{ color: colors.text }}>Settings Screen</Text>
      <Text style={{ color: colors.text }}>
        userParam: {JSON.stringify(user)}
      </Text>
      <Button onPress={() => navigation.navigate('Profile')}>
        Go to Profile
      </Button>
    </View>
  );
}

function ProfileScreen() {
  const { colors } = useTheme();

  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Text style={{ color: colors.text }}>Profile Screen</Text>
    </View>
  );
}

// codeblock-focus-start
function MyButton() {
  // highlight-next-line
  const { colors } = useTheme();

  return (
    <TouchableOpacity style={{ backgroundColor: colors.card }}>
      <Text style={{ color: colors.text }}>Button!</Text>
    </TouchableOpacity>
  );
}
// codeblock-focus-end

function HomeScreen() {
  const navigation = useNavigation();
  const { colors } = useTheme();

  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Text style={{ color: colors.text }}>Home Screen</Text>
      <MyButton />
      <Button
        onPress={() =>
          navigation.navigate('Panel', {
            screen: 'Settings',
            params: { user: 'jane' },
          })
        }
      >
        Go to Settings
      </Button>
    </View>
  );
}

const PanelStack = createNativeStackNavigator({
  screens: {
    Profile: ProfileScreen,
    Settings: SettingsScreen,
  },
});

const MyDrawer = createDrawerNavigator({
  initialRouteName: 'Panel',
  screens: {
    Home: HomeScreen,
    Panel: {
      screen: PanelStack,
      options: {
        headerShown: false,
      },
    },
  },
});

const Navigation = createStaticNavigation(MyDrawer);

export default function App() {
  const scheme = useColorScheme();
  return <Navigation theme={scheme === 'dark' ? DarkTheme : DefaultTheme} />;
}
```

**Dynamic:**

```js name="useTheme hook" snack
import * as React from 'react';
import {
  useNavigation,
  DefaultTheme,
  DarkTheme,
  NavigationContainer,
} from '@react-navigation/native';
import { View, Text, TouchableOpacity, useColorScheme } from 'react-native';
import { createNativeStackNavigator } from '@react-navigation/native-stack';
import { createDrawerNavigator } from '@react-navigation/drawer';
import { Button } from '@react-navigation/elements';
// codeblock-focus-start
import { useTheme } from '@react-navigation/native';

// codeblock-focus-end

function SettingsScreen({ route }) {
  const navigation = useNavigation();
  const { user } = route.params;
  const { colors } = useTheme();

  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Text style={{ color: colors.text }}>Settings Screen</Text>
      <Text style={{ color: colors.text }}>
        userParam: {JSON.stringify(user)}
      </Text>
      <Button onPress={() => navigation.navigate('Profile')}>
        Go to Profile
      </Button>
    </View>
  );
}

function ProfileScreen() {
  const { colors } = useTheme();

  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Text style={{ color: colors.text }}>Profile Screen</Text>
    </View>
  );
}

// codeblock-focus-start
function MyButton() {
  // highlight-next-line
  const { colors } = useTheme();

  return (
    <TouchableOpacity style={{ backgroundColor: colors.card }}>
      <Text style={{ color: colors.text }}>Button!</Text>
    </TouchableOpacity>
  );
}
// codeblock-focus-end

function HomeScreen() {
  const navigation = useNavigation();
  const { colors } = useTheme();

  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Text style={{ color: colors.text }}>Home Screen</Text>
      <MyButton />
      <Button
        onPress={() =>
          navigation.navigate('Panel', {
            screen: 'Settings',
            params: { user: 'jane' },
          })
        }
      >
        Go to Settings
      </Button>
    </View>
  );
}

const Stack = createNativeStackNavigator();

function PanelStack() {
  return (
    <Stack.Navigator>
      <Stack.Screen name="Profile" component={ProfileScreen} />
      <Stack.Screen name="Settings" component={SettingsScreen} />
    </Stack.Navigator>
  );
}

const Drawer = createDrawerNavigator();

function MyDrawer() {
  return (
    <Drawer.Navigator initialRouteName="Panel">
      <Drawer.Screen name="Home" component={HomeScreen} />
      <Drawer.Screen
        name="Panel"
        component={PanelStack}
        options={{
          headerShown: false,
        }}
      />
    </Drawer.Navigator>
  );
}

export default function App() {
  const scheme = useColorScheme();
  return (
    <NavigationContainer theme={scheme === 'dark' ? DarkTheme : DefaultTheme}>
      <MyDrawer />
    </NavigationContainer>
  );
}
```

See [theming guide](themes.md) for more details and usage guide around how to configure themes.

## Using with class component

You can wrap your class component in a function component to use the hook:

```js
class MyButton extends React.Component {
  render() {
    // Get it from props
    const { theme } = this.props;
  }
}

// Wrap and export
export default function (props) {
  const theme = useTheme();

  return <MyButton {...props} theme={theme} />;
}
```
