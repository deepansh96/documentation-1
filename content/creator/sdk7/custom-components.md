---
date: 2018-01-15
title: Custom components
description: Create a custom component to handle specific data related to an entity
categories:
  - development-guide
type: Document
aliases:
  - /development-guide/custom-components/
url: /creator/development-guide/custom-components/
---

Data about an entity is stored in its [components](/creator/development-guide/entities-components). The Decentraland SDK provides a series of base components that manage different aspects about an entity, like its position, shape, material, etc. The engine knows how to interpret the information in these, and will change how the entity is rendered accordingly as soon as they change their values.

If your scene's logic requires storing information about an entity that isn't handled by the default components of the SDK, then you can create a custom type of component on your scene. You can then build [systems](/creator/development-guide/systems) that check for changes on these components and respond accordingly.


## About defining components

To define a new component, use `engine.defineComponent`. Each component needs the following:

- A **schema**: A class that defines the data structure held by the component.
- An **ID**: A unique identifier that the SDK uses to identify this component type.

```ts
export const WheelSpinComponent = engine.defineComponent({
	spinning: Schemas.Boolean,
	speed: Schemas.Float
}, 2000)
```

> Tip: Define your scene's custom components in a separate `/components` folder inside `/src`, each in its own file. That way it's easier to reuse these in future projects.


Once you defined a custom component, you can create instances of this component, that reference entities in the scene. When you create an instance of a component, you provide values to each of the fields in the component's schema. The values must comply with the declared types of each field.

```ts
// Create entities
const wheel = engine.addEntity()
const wheel2 = engine.addEntity()

// Create instances of the component
WheelSpinComponent.create(wheel1, {
	spinning: true,
	speed: 10	
})

WheelSpinComponent.create(wheel2, {
	spinning: false,
	speed: 0	
})
```

Each entity that has the component added to it instances a new copy of the component, holding specific data for that entity.

Your custom component can also perform the other common functions that are available on other components:

```ts
// Fetch a read only instance of the component from an entity
const readOnlyInstance MyCustomComponent.get(myEntity)

// Fetch a mutable instance of the component from an entity
const readOnlyInstance MyCustomComponent.getMutable(myEntity)

// Delete an entity's instance of the component
const readOnlyInstance MyCustomComponent.deleteFrom(myEntity)

```


## About the ID

Each component must have a unique ID, that differentiates it internally. The Decentraland SDK has the numbers 1000 to 2000 reserved for the base components. Any new component you create must have an id that is a number higher than **2000**.

To easily create new ids....

<!-- TODO: tips -->


> Warning: Component IDs must be created in a deterministic way. Do not give it a random number, or one that may vary based on in what order the scene loads, or the actions taken by the player. This is especially consideration if changes in the scene are synced between players. If two player's local versions of the scene use different ids for a same component, it won't be possible to properly sync them.


## Components as flags

You may want to add a component that simply flags an entity to differentiate it from others, without using it to store any data. To do this, leave the schema as an empty object.

This is especially useful when using [querying components](/creator/development-guide/querying-components). A simple flag component can be used tell entities apart from others, and avoid having the system iterate over more entities than needed.

```ts
export const IsEnemyFlag = engine.defineComponent({}, 2000)
```

## Component Schemas

A schema describes the structure of the data inside a component. A component can store as many fields as you want, each one must be included in the schema's structure. The schema can include as many levels of nested items as you need.

Every field in the schema must include a type declaration. You can only use the special schema types provided by the SDK. For example, use the type `Schemas.Boolean` instead of type `boolean`. Write `Schemas.` and your IDE will display all the available options.


```ts
export const WheelSpinComponent = engine.defineComponent({
	spinning: Schemas.Boolean,
	speed: Schemas.Float
}, 2000)
```

The example above defines a component who's schema holds two values, a `spinning` boolean and a `speed` floating point number. 


You can chose to create the schema inline while defining the component, or for more legibility you can create it and then reference it.

```ts
// Option 1: Inline definition
export const WheelSpinComponent = engine.defineComponent({
	spinning: Schemas.Boolean,
	speed: Schemas.Float
}, 2000)

// Option 2: define schema and component separately

//// schema
const mySchema = {
	spinning: Schemas.Boolean,
	speed: Schemas.Float
}

//// component
export const WheelSpinComponent = engine.defineComponent(mySchema, 2000)
```

> Tip: When creating an instance of a component, the VS Studio autocomplete options will suggest what fields you can add to the component by pressing _Ctrl + Space_.

> Note: All values in a custom component are optional when instancing a component. There is no mechanism to define default values for these fields when instancing the component, but you can define systems that execute default behaviors if no values are present for a given field.

### Basic Schema types

The following types are available for using within the fields of a schema: 

- `Scehmas.Boolean`
- `Scehmas.Byte`
- `Scehmas.Double`
- `Scehmas.Float`
- `Scehmas.Int`
- `Scehmas.Int64`
- `Scehmas.Number`
- `Scehmas.Short`
- `Scehmas.String`


### Complex schema types

The types in a schema can also be arrays or objects with other nested fields.

To set the type of a field as an array, use `Schemas.Array()`. Pass the type of the elements in the array as a property.

```ts
const MySchema = {
	numberList: Schemas.Array(Schemas.Int),
  }
```

To set the type of a field to be an object, use `Schemas.Map()`. Pass the contents of this object as a property. This nested object is essentially a schema itself, nested within the parent schema.


```ts
const MySchema = {
	simpleField: Schemas.Boolean,
	myComplexField: Schemas.Map({
		nestedField1: Schemas.Boolean,
		nestedField2: Schemas.Boolean			
	})}
}
```

Alternatively, to keep things more readable and reusable, you could achieve the same by defining the nested schema separately, then referencing it when defining the parent schema.

```ts
const MyNestedSchema = Schemas.Map({
		nestedField1: Schemas.Boolean,
		nestedField2: Schemas.Boolean			
	})

const MySchema = {
	simpleField: Schemas.Boolean,
	myComplexField: MyNestedSchema
}
```



### Enums types

You can set the type of a field in a schema to be an enum. Enums make it easy to select between a finite number of options, providing human-readable values for each.

To set the type of a field to an enum, you must first define the enum. Then you can refer to it using `Schemas.Enum`, passing the enum to reference between `<>`, as well as the type as a parameter (usually all enums are `Schemas.Int`).

```ts
// Define an enum
enum CurveType {
  LINEAR,
  EASEIN,
  EASEOUT
}

// Define a schema that uses this enum in a field
const MyCurveSchema = {
  curve:  Schemas.Enum<CurveType>(Schemas.Int),
}

```

## Building systems to use a component

With your component defined and added to entities in your scene, you can create systems to perform logic, making use of this data stored on the component.

```ts
// define component
export const WheelSpinComponent = engine.defineComponent({
	spinning: Schemas.Boolean,
	speed: Schemas.Float
}, 2000)

// Create entities
const wheel = engine.addEntity()
const wheel2 = engine.addEntity()

// Create instances of the component
WheelSpinComponent.create(wheel1, {
	spinning: true,
	speed: 10	
})

WheelSpinComponent.create(wheel2, {
	spinning: false,
	speed: 0	
})

// Define a system to iterate over these entities
export function spinSystem(dt: number) {
	// iterate over all entiities with a WheelSpinComponent
  for (const [entity, wheelSpin] of engine.getEntitiesWith(WheelSpinComponent)) {

	// only do something if spinning == true
	if(wheelSpin.spinning){

		// fetch a mutable Transform component 
		const transform = Transform.getMutable(entity)

		// update the rotation value accordingly
		transform.rotation = Quaternion.multiply(transform.rotation, Quaternion.angleAxis(dt * wheelSpin.speed, Vector3.Up()))
	}
  }
}

// Add system to engine
engine.addSystem(spinSystem)
```

The example above defines a system that iterates over all entities that include the custom `WheelSpinComponent`, and rotates them slightly on every tick of the game loop. The amount of this rotation is proportional to the `speed` value stored on each entity's instance of the component. The example makes use of [component queries](/creator/development-guide/querying-components) to obtain only the relevant entities.