# Section 1: Getting Started
- Steps to make android app:
  1. create screen dir and create all screen js
  2. create components for screen -> 
  3. create models
# Section 2: React Native Basics
## 22. Handling Events
- Different between:
    ```js
    // with paranthesis
    // it will be excuted as soon as file rendered
    onChangeText={goalInputHandler()}
    //// without paranthesis
    // will be executed while triggered
    onChangeText={goalInputHandler}
    ```
## 24. iOS & Android Styling Differences
- There is no inheritance and cascading in styling react native like css do
- Sometimes, there is different styling between android and iOS. Exp:
```js
// there is no borderRadius in iOS Text
<Text style={styles.goalItem} key={goal}>{goal}</Text>
// to fix this, wrap Text into View
<View style={styles.goalItem}>
    <Text style={styles.goalText} key={goal}>{goal}</Text>
</View>

// styling
const styles = StyleSheet.create({
  goalItem: {
    margin: 8,
    padding: 8,
    borderRadius: 6,
    backgroundColor: '#5e0acc',    
  },
  goalText: {
    color: 'white'
  }  
})
```
## 25. Making Content Scrollable with ScrollView
```js
  <View style={styles.goalsContainer}>
    <ScrollView>
      {courseGoals.map((goal) => (
        <View style={styles.goalItem} key={goal}>
          <Text style={styles.goalText}>{goal}</Text>
        </View>
      ))}        
    </ScrollView>
  </View>
```
## 26. Optimizing List with FlatList
- Problem using Scrollable for list: `ScrollView` renders its all child. For very long list, this can be performance issue.
- Solution FlatList 1: Without Using Key Extractor
  ```js
  // State
  const [enteredGoalText, setEnteredGoalText] = useState('')
  const [courseGoals, setCourseGoals] = useState([])

  function goalInputHandler(enteredText) {
    setEnteredGoalText(enteredText)
  }

  function addGoalHandler() {
    setCourseGoals(
      (currentCourseGoals) => [
        ...currentCourseGoals, 
        enteredGoalText
      ])
  }    

    // FlatList
    <View style={styles.goalsContainer}>
      <FlatList 
        data={courseGoals} 
        renderItem={(itemData) => {
          return(
            <View style={styles.goalItem} key={itemData.index}>
              <Text style={styles.goalText}>{itemData.item}</Text>
            </View>                  
          )
        }} 
      />
    </View>
  ```
- Solution FlatList 2: Using Key Extractor
  ```js
    //State
    const [enteredGoalText, setEnteredGoalText] = useState('')
    const [courseGoals, setCourseGoals] = useState([])

    function goalInputHandler(enteredText) {
      setEnteredGoalText(enteredText)
    }

    function addGoalHandler() {
      setCourseGoals(
        (currentCourseGoals) => [
          ...currentCourseGoals, 
          {text: enteredGoalText, id: Math.random().toString()}
        ])
    }

    // FlatList
    <View style={styles.goalsContainer}>
      <FlatList 
        data={courseGoals} 
        renderItem={(itemData) => {
          return(
            <View style={styles.goalItem}>
              <Text style={styles.goalText}>{itemData.item.text}</Text>
            </View>                  
          )          
        }} 
        keyExtractor={(item, index) => {
          return item.id
        }}
      />
    </View>  
  ```
## 27, 28, 29, 30, 31, 32
- Splitting Components Into Smaller Components, Props binding, filter, Communication between parent and children components, Object Destructuring
- Button doesnt have style, use `Pressable` to custom button
- App.js
  ```js
  import { useState } from 'react';
  import { 
    StyleSheet, 
    View, 
    FlatList 
  } from 'react-native';


  import GoalItem from './components/GoalItem';
  import GoalInput from './components/GoalInput';


  export default function App() {  
    const [courseGoals, setCourseGoals] = useState([])

    function addGoalHandler(enteredGoalText) {
      setCourseGoals(
        (currentCourseGoals) => [
          ...currentCourseGoals, 
          {text: enteredGoalText, id: Math.random().toString()}
        ])
    }

    function deleteGoalHandler(id) {
      setCourseGoals((currentCourseGoals) => {
        return currentCourseGoals.filter((goal) => goal.id !== id)
      })    
    }


    return (
      <View style={styles.appContainer}>
        
        {/* pointer fungsi addGoalHandler ke GoalInput melalui prop onAddGoal */}
        <GoalInput onAddGoal={addGoalHandler} />

        <View style={styles.goalsContainer}>
          <FlatList 
            data={courseGoals} 
            renderItem={(itemData) => {
              return(
                <GoalItem 
                  text={itemData.item.text} 
                  id={itemData.item.id} 
                  onDeleteItem={deleteGoalHandler} 
                />
              )          
            }} 
            keyExtractor={(item, index) => {
              return item.id
            }}
          />
        </View>
      </View>
    );
  }


  const styles = StyleSheet.create({
    appContainer: {
      flex: 1,
      paddingTop: 50,
      paddingHorizontal: 16
    },
    goalsContainer: {
      flex: 5
    },
  });

  ```
- GoalInput.js
  ```js
  import { useState } from 'react'
  import {StyleSheet, View, Button, TextInput} from 'react-native'

  function GoalInput(props) {
      const [enteredGoalText, setEnteredGoalText] = useState('')

      function goalInputHandler(enteredText) {
          setEnteredGoalText(enteredText)
      }   
      
      function addGoalHandler() {
          // manggil fungsi pointer yang dilempar dari props
          props.onAddGoal(enteredGoalText)
          setEnteredGoalText('')
      }

      return(
          <View style={styles.inputContainer}>
              <TextInput 
                  style={styles.textInput} 
                  placeholder="Your course goal!" 
                  onChangeText={goalInputHandler}
                  value={enteredGoalText}
              />
              <Button 
                  title="Add Goal" 
                  onPress={addGoalHandler}
              />
        </View>        
      )
  }


  export default GoalInput

  const styles = StyleSheet.create({
      inputContainer: {
          flex: 1,
          flexDirection: 'row',
          justifyContent: 'space-between',
          alignItems: 'center',
          marginBottom: 24,
          borderBottomWidth: 1,
          borderBottomColor: '#cccccc'
        },
        textInput: {
          borderWidth: 1,
          borderColor: '#cccccc',
          width: '70%',
          padding: 8
        },
  })
  ```
- GoalItem.js
  ```js
  import {StyleSheet, View, Text, Pressable} from 'react-native'

  function GoalItem(props) {

      return(
          <View style={styles.goalItem}>
              {/* Disini pakai bind, bukan pakai fungsi, pakai fungsi bisa kayak addGoalHandler, ini cara lainnya */}
              <Pressable 
                  onPress={props.onDeleteItem.bind(this, props.id)}
                  // ripple
                  android_ripple={{color: '#210644'}}
                  // untuk styling ripple pada apple
                  // ({pressed}) adalah object destructuring
                  style={({pressed}) => pressed && styles.pressedItem}
                  // cara tanpa destructuring
                  // style={(pressedData) => pressedData.pressed && styles.pressedItem}
              >
                  <Text style={styles.goalText}>{props.text}</Text>
              </Pressable>
          </View>                  
      )
  }


  export default GoalItem

  const styles = StyleSheet.create({
      goalItem: {
          margin: 8,        
          borderRadius: 6,
          backgroundColor: '#5e0acc',    
      },
      pressedItem: {
          opacity:0.5
      },
      goalText: {
          padding: 8,
          color: 'white'
      }
  })
  ```
## 33-38
- add this code to app.json to style all backgroud by default
  ```js
  "backgroundColor": "#1e085a",
  ```
- add StatusBar to style StatusBar
  ```js
  <StatusBar style='light' />
  ```

# Section 3: Debugging React Native Apps
# Section 4: Diving Dipper Into Components, Layouts & Styling - Building Mini Game App
## 45-53
- Button component in react-native actually consists of View and Text
- Making Shadow in android and Ios
  ```js
    elevation: 4, //only for android
    // shadow is for ios
    shadowColor: 'black',
    shadowOffset: {width:0, height:2},
    shadowRadius: 6,
    shadowOpacity: 0.25,
  ```
- children merupakan props.children. Standar react untuk mengambil semua isi diantara tag pembuka dan tag penutup
- riple only for android, in Ios it must be manual setting
  ```js
  // children merupakan props.children. Standar react untuk mengambil semua isi diantara tag pembuka dan tag penutup
  function PrimaryButton({children, onPress}) {    

      return(
          <View style={styles.buttonOuterContainer}>
              <Pressable 
                  // untuk ios dan android, pressed adalah props property milik Pressable dari react native
                  style={({pressed}) => pressed ? [styles.buttonInnerContainer, styles.pressed]: styles.buttonInnerContainer} 
                  onPress={onPress} 
                  android_ripple={{color:  '#640233'}}
              >
                  <Text style={styles.buttonText}>{children}</Text>
              </Pressable>
          </View>
      )
  }


  const styles = StyleSheet.create({
      buttonOuterContainer: {
          borderRadius: 28,
          margin: 4,
          overflow: 'hidden',
      },
      buttonInnerContainer: {
          backgroundColor: "#72063c",        
          paddingVertical: 8,
          paddingHorizontal: 16,        
          elevation: 2 //hanya di android
      },
      buttonText: {
          color: 'white',
          textAlign: 'center'
      },
      pressed: {
          opacity: 0.75,        
      }

  })
  ```
## 54-66
- UseEffect
  ```js
      // untuk melihat jika ada perubahan
      useEffect(() => {
          if (currentGuess === userNumber) {
              onGameOver()
          }
      // perubahan pada currentGuess atau userNumber atau onGameOver
      }, [currentGuess, userNumber, onGameOver])
  ```
- IMPORTANT! UseEffect, technically run after the component function has been executed. So we need to hard code this code to avoid error because this code run first before useEffect
  ```js
  // from
  const initialGuess = generateRandomBetween(minBoundary, maxBoundary, userNumber)
  // to
  const initialGuess = generateRandomBetween(1, 100, userNumber)
  ```
# 68. Using "Casacading Styles" 
- No Cascading Style in react-native because we dont use css here, so to make like cascading style, use this trick
  ```js  
  // 1. add prop to component and we name it "style"
  function InstrunctionText({children, style}) {
      return(
          // look at this one
          // 2. use array[] so we can merge style
          <Text style={[styles.instructionText, style]}>{children}</Text>
      )    
  }

  export default InstrunctionText

  const styles = StyleSheet.create({
      instructionText: {
          color: Colors.accent500,
          fontSize: 24,
      },
  })

  //3 How to use it
  // styles.InstructionText will merge with style in parent component and if there is same object, old style object will be overrided
  <InstructionText style={styles.InstructionText}>Higher or lower?</InstructionText>                

  const styles = StyleSheet.create({      
      InstructionText: {
          marginBottom: 12
      },      
  })
  ```
# 69, 70 
- Icon: `import {Ionicons} from '@expo/vector-icons'`
- Font and Splash:
  ```js
  import { useState, useEffect, useCallback } from 'react';
  import { StyleSheet, ImageBackground, SafeAreaView } from 'react-native';
  import {LinearGradient} from 'expo-linear-gradient';
  import * as Font from 'expo-font';
  import * as SplashScreen from 'expo-splash-screen';

  import StartGameScreen from './screens/StartGameScreen';
  import GameScreen from './screens/GameScreen';
  import GameOverScreen from './screens/GameOverScreen';
  import Colors from './constants/color';



  export default function App() {
    const [appIsReady, setAppIsReady] = useState(false);
    const [userNumber, setUserNumber] = useState()
    const [gameIsOver, setGameIsOver] = useState(true)

    const fontsLoading = {
      'open-sans': require('./assets/fonts/OpenSans-Regular.ttf'),
      'open-sans-bold': require('./assets/fonts/OpenSans-Bold.ttf')
    }

    useEffect(() => {
      async function prepare() {
        try {
            await SplashScreen.preventAutoHideAsync();
            await Font.loadAsync(fontsLoading);
        } catch (e) {
            console.warn(e);
        } finally {
            setAppIsReady(true);
        }
      }

      prepare();
  }, []);

  const onLayoutRootView = useCallback(
      async () => {
        if (appIsReady) {
            await SplashScreen.hideAsync();
        }
      },
      [ appIsReady ]
  );

  if (!appIsReady) {
      return null;
  }

    function pickedNumberHandler(pickedNumber){
      setUserNumber(pickedNumber)
      setGameIsOver(false)
    }

    function gameOverHandler() {
      setGameIsOver(true)
    }

    let screen = <StartGameScreen onPickedNumber={pickedNumberHandler} />

    if (userNumber) {
      screen = <GameScreen userNumber={userNumber} onGameOver={gameOverHandler} />
    }

    if (gameIsOver && userNumber) {
      screen = <GameOverScreen></GameOverScreen>
    }


    return (
      <LinearGradient 
        colors={[Colors.primary700, Colors.accent500]} 
        style={styles.rootScreen}
        onLayout={onLayoutRootView}
      >
        <ImageBackground source={require('./assets/images/background.png')} resizeMode="cover" style={styles.rootScreen} imageStyle={styles.backgroundImage}>
          {/* SafeAreaView agar aman */}
          <SafeAreaView style={styles.rootScreen}>
            {screen}
          </SafeAreaView>
        </ImageBackground>
      </LinearGradient>
    );
  }

  const styles = StyleSheet.create({
    rootScreen: {
      flex: 1,        
    },
    backgroundImage: {
      opacity: 0.15,
    }
  });
  ```
# 72. Using and Styling Nested Text
- Nested in react native doesnt work, except for Text
  ```js
  <Text style={styles.summaryText}>Your phone needed <Text style={styles.hightlight}>X</Text> rounds to guess the number <Text style={styles.hightlight}>Y</Text> </Text>            
  ```

# Section 5: Building Adaptive User Interfaces (Adapt to Platform & Device Sizes)
## 80. Setting Dynamic Width
- Combine maxWidth/minWidth with regular pixel width
  ```js
  // in this case, make it 300 pixel (default value) or in smaller device, make it 80% of screen
  maxWidth: '80%',
  width: 300,
  ```
## 81. Introduction the Dimensions API
```js
import { View, Text, StyleSheet, Dimensions } from "react-native";

const deviceWidth = Dimensions.get('window').width

const styles = StyleSheet.create({
    container: {  
        padding: deviceWidth < 380 ? 12 : 24,        
        margin: deviceWidth < 380 ? 12 : 24,        
    },
    NumberText: {    
        fontSize: deviceWidth < 380 ? 28 : 36,        
    }
})
```
## 82, 83, 84, 85, 86 Setting Orientation in Potrait and Landscape
- set app to make it dynamically potrait or landscape: in app.json:
  ```js
  "orientation": "default",
  ```
- When we rotate from potrait and landscape, we cant use Dimensions located in hard file code because its only executed once when the component mounted, use Hook instead to make it dynamic
  ```js
  // import
  import { TextInput, View, StyleSheet, Alert, useWindowDimensions } from "react-native"

  // get DImension
  const {width, height} = useWindowDimensions()
  // margin
  const marginTopDistance = height < 380 ? 30 : 100
  // add in style
  <View style={[styles.rootContainer, {marginTop: marginTopDistance}]}>
  ```
- Managing Screen Content with KeyboardAvoidingView
  ```js
  <ScrollView style={styles.screen}>
      <KeyboardAvoidingView style={styles.screen} behavior="position">
          <View>
              //isinya disini
          </View>
      </KeyboardAvoidingView>
  </ScrollView>
  ```
- Make different style in landscape and potrait: get width using Dimensions API
  ```js
    // style 1  
    let content = <> 
                    <NumberContainer>{currentGuess}</NumberContainer>            
                    <Card>
                        <InstructionText style={styles.InstructionText}>Higher or lower?</InstructionText>                
                        <View style={styles.buttonsContainer}>
                            <View style={styles.buttonContainer}>
                                <PrimaryButton onPress={nextGuessHandler.bind(this, 'lower')}>
                                    <Ionicons name="md-remove" size={24} color="white" />
                                </PrimaryButton>
                            </View>
                            <View style={styles.buttonContainer}>
                                <PrimaryButton onPress={nextGuessHandler.bind(this, 'greater')}>
                                    <Ionicons name="md-add" size={24} color="white" />
                                </PrimaryButton>
                            </View>
                        </View>                
                    </Card>    
                </>

    // style 2
    if (width > 500) { // disini pointnya
        content = <>                    
                    <View style={styles.buttonContainerWide}>                        
                        <View style={styles.buttonContainer}>
                            <PrimaryButton onPress={nextGuessHandler.bind(this, 'lower')}>
                                <Ionicons name="md-remove" size={24} color="white" />
                            </PrimaryButton>
                        </View>
                        <NumberContainer>{currentGuess}</NumberContainer>            
                        <View style={styles.buttonContainer}>
                            <PrimaryButton onPress={nextGuessHandler.bind(this, 'greater')}>
                                <Ionicons name="md-add" size={24} color="white" />
                            </PrimaryButton>
                        </View>                        
                    </View>
                </>
    }

    return(
        <View style={styles.screen}>                      
            <Title>Opponent's Guess</Title>            
            {content} // peletakan beda stylenya
            <View  style={styles.listContainer}>                
                <FlatList                     
                    data={guessRounds} 
                    renderItem={(itemData) => (
                        <GuessLogItem 
                            roundNumber={guessRoundsListLength - itemData.index} 
                            guess={itemData.item}
                        />
                    )}
                    keyExtractor={(item) => item}
                />
            </View>
        </View>
    )
  ```
## 88 Platform API to write platform-specific Code 
- There is 3 way
  ```js
  // cara 1
  borderWidth: Platform.OS === 'android' ? 0: 2,
  // cara 2
  borderWidth: Platform.select({ios:2, android: 0}),
  // cara 3 - different file:
  //- Create 2 file: for android name it with ComponentName.android.js, for ios name it with ComponentName.ios.js
  ```
## 89 Styling the StatusBar
```js
import { StatusBar } from 'expo-status-bar';

<StatusBar style='light' />
```

# Section 6
## 90-96 Basic Navigation
1. In App.js
  ```js
  import { NavigationContainer } from '@react-navigation/native';
  import { createNativeStackNavigator } from '@react-navigation/native-stack';
  import { StatusBar } from 'expo-status-bar';
  import { StyleSheet, Text, View } from 'react-native';

  import CategoriesScreen from './screens/CategoriesScreen';
  import MealsOverviewScreen from './screens/MealsOverviewScreen';


  const Stack = createNativeStackNavigator()

  export default function App() {
    return (
      <>
      <StatusBar style='dark' />
        // 1. All navigation located here, the top-most screen is used as the initial screen.
        <NavigationContainer>
          <Stack.Navigator>
            <Stack.Screen name='MealsCategories' component={CategoriesScreen} />
            <Stack.Screen name="MealsOverview" component={MealsOverviewScreen} />
          </Stack.Navigator>        
        </NavigationContainer>
      </>
    );
  }

  const styles = StyleSheet.create({
    container: {
      flex: 1,
      backgroundColor: '#fff',
      alignItems: 'center',
      justifyContent: 'center',
    },
  });

  ```
2. In CategoriesScreen
  ```js
  import {FlatList } from "react-native"
  import CategoryGridTitle from "../components/CategoryGridTitle"

  import {CATEGORIES} from '../data/dummy-data'


  function CategoriesScreen({navigation}) {

      function renderCategoryItem(itemData) {
          // navigate to certain screen
          function pressHandler() {
              navigation.navigate('MealsOverview') 
          }

          return <CategoryGridTitle 
                  title={itemData.item.title} 
                  color={itemData.item.color} 
                  // when onpress
                  onPress={pressHandler} 
                  />
      }

      
      return(
          <FlatList
              data={CATEGORIES}
              keyExtractor={(item) => item.id}
              renderItem={renderCategoryItem}
              numColumns={2}
          />
      )
  }


  export default CategoriesScreen
  ```
## 97-98
- There is 2 way to pass `navigation` and `route` in Navigation. Pass it using props or use hook.
- `navigation` is used to navigate and pass value to new screen, and `route` is used to get the value from previous screen.
- If we use props, the component must be children of `<Stack.Navigator>` or pass by it's children 
- Example of `navigation`:
  ```js
  // in app.js
  <NavigationContainer>
    <Stack.Navigator>
      <Stack.Screen name='MealsCategories' component={CategoriesScreen} />
      <Stack.Screen name="MealsOverview" component={MealsOverviewScreen} />
    </Stack.Navigator>        
  </NavigationContainer>

  // in CategoriesScreen
  // using props =================================================================
  // look at this props navigation
  function CategoriesScreen({navigation}) {

      function renderCategoryItem(itemData) {
          function pressHandler() {
              // use navigation here
              navigation.navigate('MealsOverview', {
                  categoryId: itemData.item.id,                
              }) 
          }

          return <CategoryGridTitle 
                  title={itemData.item.title} 
                  color={itemData.item.color} 
                  onPress={pressHandler} 
                  />
      }

      
      return(
          <FlatList
              data={CATEGORIES}
              keyExtractor={(item) => item.id}
              renderItem={renderCategoryItem}
              numColumns={2}
          />
      )
  }


  // using useNavigation Hook ====================================================
  import { useNavigation } from "@react-navigation/native"
  import {FlatList } from "react-native"
  import CategoryGridTitle from "../components/CategoryGridTitle"

  import {CATEGORIES} from '../data/dummy-data'


  function CategoriesScreen() {
    //lokk at this hook
      const navigation = useNavigation()


      function renderCategoryItem(itemData) {
          function pressHandler() {
              navigation.navigate('MealsOverview', {
                  categoryId: itemData.item.id,                
              }) 
          }

          return <CategoryGridTitle 
                  title={itemData.item.title} 
                  color={itemData.item.color} 
                  onPress={pressHandler} 
                  />
      }

      
      return(
          <FlatList
              data={CATEGORIES}
              keyExtractor={(item) => item.id}
              renderItem={renderCategoryItem}
              numColumns={2}
          />
      )
  }


  export default CategoriesScreen
  ```
- Example of `route`:
  ```js
  // in CategoriesScreen.js
  function renderCategoryItem(itemData) {
      function pressHandler() {
        // the value to pass to 'MealsOverview'
        navigation.navigate('MealsOverview', {
            categoryId: itemData.item.id,                
        }) 
      }

      return <CategoryGridTitle 
              title={itemData.item.title} 
              color={itemData.item.color} 
              onPress={pressHandler} 
              />
  }


  // in MealOverviewScreen.js
  // using props route ==================================================
  import { View, Text, StyleSheet } from "react-native"

  // look at this route pros
  function MealsOverviewScreen({route}) {
      const catId = route.params.categoryId

      return(
          <View style={styles.container}>
              <Text>Meals Overview Screen - {catId}</Text>
          </View>
      )
  }

  // using route hook ===================================================
  import { useRoute } from "@react-navigation/native"
  import { View, Text, StyleSheet } from "react-native"


  function MealsOverviewScreen() {
      // look at this hook
      const route = useRoute()

      const catId = route.params.categoryId

      return(
          <View style={styles.container}>
              <Text>Meals Overview Screen - {catId}</Text>
          </View>
      )
  }
  ```
## 99, 100, 101, 102
- Setting Header in Parent (App.js)
  ```js
  <NavigationContainer>
    // styling for all screen header
    <Stack.Navigator 
          screenOptions={{
              headerStyle: {backgroundColor: '#351401'},
              headerTintColor: 'white',
              contentStyle: {backgroundColor: '#3f2f25'}
          }}
    >
      <Stack.Screen 
        name='MealsCategories' 
        component={CategoriesScreen} 
        // styling for specified screen header 
        options={{
          title: 'All Categories',              
        }}
      />
      <Stack.Screen 
        name="MealsOverview" 
        component={MealsOverviewScreen} 
        // options={({route, navigation}) => {
        //   const catId = route.params.categoryId
        //   return {
        //     title: catId
        //   }
        // }}
      />
    </Stack.Navigator>        
  </NavigationContainer>
  ```
- Setting in Specified Screen:
  ```js
  // use useLayoutEffect where you tipically have some kind of ongoing animation and u wanna set or execute some side effetct while this is still happening and before the component has finished executed
  useLayoutEffect(() => {        
      const categoryTitle = CATEGORIES.find((category) => category.id === catId).title

      navigation.setOptions({
          title: categoryTitle
      })
  }, [catId, navigation])
  ```
## 106 Adding Header Button
- In screen that use the header. In this case is MealDetailScreen.js
  ```js
  import { useLayoutEffect } from "react"


  function headerButtonPressHandler() {
      console.log("masuk");
  }

  useLayoutEffect(() => { 
      navigation.setOptions({            
          headerRight: () => {
              return (
              <Button title='Tap me!' onPress={headerButtonPressHandler} />
              )
          }              
      })
  }, [navigation, headerButtonPressHandler])
  ```
## 108, 109 Drawer
## 110 Bottom Tabs
## 111, 112 Nesting Navigator
- In this example we nested 2 navigators, drawer and stack navigator
  ```js
  // this is Drawer
  function DrawerNavigator() {
    return ( <Drawer.Navigator
                screenOptions={{
                  headerStyle: {backgroundColor: '#351401'},
                  headerTintColor: 'white',
                  sceneContainerStyle: {backgroundColor: '#3f2f25'},
                  drawerContentStyle: {backgroundColor: '#351401'},
                  drawerInactiveTintColor: 'white',
                  drawerActiveTintColor: '#351401',
                  drawerActiveBackgroundColor: '#e4baa1'
              }}
              >
                <Drawer.Screen 
                  name="Categories" 
                  component={CategoriesScreen}  
                  options={{
                    title: "All Categories",
                    drawerIcon: ({color, size}) => <Ionicons name="list" color={color} size={size} />  
                  }} 

                />
                <Drawer.Screen 
                  name="Favorites" 
                  component={FavoritesScreen} 
                  options={{                  
                    drawerIcon: ({color, size}) => <Ionicons name="star" color={color} size={size} />  
                  }}                 
                />
            </Drawer.Navigator>
    )
  }


  // this is Stack Navigation
  export default function App() {
    return (
      <>
      <StatusBar style='light' />
        <NavigationContainer>
          <Stack.Navigator 
                screenOptions={{
                    headerStyle: {backgroundColor: '#351401'},
                    headerTintColor: 'white',
                    contentStyle: {backgroundColor: '#3f2f25'}
                }}
          >
            <Stack.Screen 
              name='Drawer' 
              // look at this! we call drawer here
              component={DrawerNavigator} 
              options={{
                // title: 'All Categories',              
                // remove header 
                headerShown: false
              }}
            />
            <Stack.Screen 
              name="MealsOverview" 
              component={MealsOverviewScreen} 
            />          
            <Stack.Screen 
              name="MealDetail" 
              component={MealDetailScreen}
            />          
          </Stack.Navigator>        
        </NavigationContainer>
      </>
    );
  }
  ```

# Section 7 
## Managing App-Wide State with Context
1. create store -> context -> favorites-context.js
  ```js
  import {createContext, useState} from 'react'


  // this is used for better auto completion
  export const FavoritesContext = createContext({
      ids: [],
      addFavorite: (id) => {},
      removeFavorite: (id) => {}
  })

  // wrapper for components that want to get value inside this context
  function FavoritesContextProvider({children}) {
      const [favoriteMealIds, setFavoritMealIds] =  useState([]);

      function addFavorite(id) {
          setFavoritMealIds((currentFavIds) => [...currentFavIds, id] )
      }

      function removeFavorite(id) {
          setFavoritMealIds((currentFavIds) =>
              currentFavIds.filter((mealId) => mealId !== id)
          )
      }

      // value to pass
      const value ={
          ids: favoriteMealIds,
          addFavorite: addFavorite,
          removeFavorite: removeFavorite,
      }

      return <FavoritesContext.Provider value={value}>{children}</FavoritesContext.Provider>
  }

  export default FavoritesContextProvider
  ```
2. Wrap all components that wanna use the context inside context Provider
  ```js
  <>
    <StatusBar style="light" />
    <FavoritesContextProvider>
      // put inside here    
    </FavoritesContextProvider>
  </>
  ```
3. Using Context in MealsDetailScreen
  ```js
  import { useContext, useLayoutEffect } from 'react';
  import { FavoritesContext } from '../store/context/favorites-context';


    const favoriteMealsCtx = useContext(FavoritesContext)

    const mealId = route.params.mealId;

    const selectedMeal = MEALS.find((meal) => meal.id === mealId);

    const MealIsFavorite = favoriteMealsCtx.ids.includes(mealId)

    function changeFavoriteStatusHandler() {
      if (MealIsFavorite) {
        favoriteMealsCtx.removeFavorite(mealId)
      } else {
        favoriteMealsCtx.addFavorite(mealId)
      }
    }
  ```
# Section 8 

# Section 9 Handling User Input
- pass props
- create validation
# Section 10 Sending Http Requests
- 
# Section 11 User Authentication
# Section 12 Using Native Devide Features(Camera, Location and More)

