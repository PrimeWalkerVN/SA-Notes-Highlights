<h1 align="center">Event Sourcing</h1>

<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#what-is-event-sourcing">What is Event Sourcing</a>
    </li>
    <li>
      <a href="#benefits-of-event-sourcing">Benefits of Event Sourcing</a>
    </li>
    <li>
      <a href="#how-does-it-work">How Does it Work</a>
    </li>
    <li><a href="#example-use-case">Example Use Case</a></li>
    <li><a href="#limitations-of-event-sourcing">Limitations of Event Sourcing</a></li>
    <li>
      <a href="#event-sourcing-architecture">Event Sourcing Architecture</a>
    </li>
    <li>
      <a href="#golang-implementation">Golang Implementation</a>
    </li>
  </ol>
</details>
<br>

## What is Event Sourcing
Event Sourcing is an architectural pattern that stores the state changes of a system as a sequence of events.
This approach allows for a complete history of changes to be preserved, which can then be used for auditing, debugging, and recovery.

Event sourcing is a design pattern in which every change to the state of a system is captured as a sequence of events. The events are stored in an append-only store and can be used to recreate the state of the system at any given time. The state of the system is reconstructed by replaying the events from the beginning.

Event sourcing can be used in various applications and systems, such as financial systems, e-commerce platforms, and workflow management systems, to name a few. In these systems, the state changes are typically complex and frequent, making it challenging to maintain consistency and integrity of the data. Event sourcing provides a robust and scalable solution to this challenge by capturing all changes as events, making it easier to maintain the consistency and integrity of the data.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Benefits of Event Sourcing
Provides a clear, auditable history of changes to the system state.
Enables the recreation of past system states.
Facilitates the development of new features based on past events.
Supports collaboration and coordination between multiple teams and microservices.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## How Does it Work
The system records every state change as an event.
The events are stored in an event store, which is a database optimized for append-only operations.
The current state of the system can be reconstructed by replaying the events from the event store.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Example Use Case
A banking system where each transaction (e.g. deposit, withdrawal) is recorded as an event. The current balance of a user's account can be calculated by replaying all of the events related to that account.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Limitations of Event Sourcing
Increased storage requirements.
Higher complexity in handling and querying the event data.
Requires additional processing power to reconstruct the system state from events.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Event Sourcing Architecture
The architecture of an event sourcing system includes a data store that stores the events, a stream processor that processes the events, and a client application that consumes the events.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Golang Implementation
```
package main

import (
	"fmt"
	"time"
)

// Event is the structure of an event in the system
type Event struct {
	ID      int64
	Event   string
	Payload interface{}
	Time    time.Time
}

// UserAccount is the structure of a user account
type UserAccount struct {
	ID        int64
	FirstName string
	LastName  string
	Username  string
	Email     string
	Password  string
	Events    []Event
}

// AddEvent adds an event to the UserAccount's event stream
func (u *UserAccount) AddEvent(event Event) {
	u.Events = append(u.Events, event)
}

// EventStore is a store that holds all the events in the system
type EventStore struct {
	Events []Event
}

// SaveEvent saves an event to the EventStore
func (es *EventStore) SaveEvent(event Event) {
	es.Events = append(es.Events, event)
}

// GetEventsForUserAccount returns all events for a user account from the EventStore
func (es *EventStore) GetEventsForUserAccount(userID int64) []Event {
	var events []Event
	for _, event := range es.Events {
		if event.Payload.(UserAccount).ID == userID {
			events = append(events, event)
		}
	}
	return events
}

func main() {
	eventStore := &EventStore{}

	user := &UserAccount{
		ID:        1,
		FirstName: "John",
		LastName:  "Doe",
		Username:  "johndoe",
		Email:     "johndoe@example.com",
		Password:  "password",
	}

	event := Event{
		ID:      1,
		Event:   "UserCreated",
		Payload: *user,
		Time:    time.Now(),
	}

	user.AddEvent(event)
	eventStore.SaveEvent(event)

	fmt.Println("Event Stream for User Account:")
	for _, event := range user.Events {
		fmt.Println("Event:", event.Event)
		fmt.Println("Payload:", event.Payload)
		fmt.Println("Time:", event.Time)
		fmt.Println("---")
	}

	fmt.Println("Events in Event Store for User Account:")
	events := eventStore.GetEventsForUserAccount(user.ID)
	for _, event := range events {
		fmt.Println("Event:", event.Event)
		fmt.Println("Payload:", event.Payload)
		fmt.Println("Time:", event.Time)
		fmt.Println("---")
	}
}

```

In this code, we have two structures: `UserAccount` and `EventStore`. The `UserAccount` structure holds information about a user account and also has an event stream

The `Event` type represents a state change in the system, and the Store type represents the event store where the events are stored. The `AddEvent` function is used to add a new event to the store, and the `GetEvents` function is used to retrieve all events from the store.

In the `main` function, an instance of the `Store` type is created, and three events are added to the store using the `AddEvent` function. Finally, all events in the store are retrieved using the `GetEvents` function, and the event details are printed to the console.

<p align="right">(<a href="#readme-top">back to top</a>)</p>