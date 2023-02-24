# Eventing

> Event-driven programming is an approach in which the flow of the program is determined 
by events such as user actions, sensor outputs, or message passing from other programs or threads

The idea of this project is that everything is, or can be, **event-driven**. 
Event sourcing is not provided by default.

## Create a new event

```php showLineNumbers title="UserWasCreated.php"
class UserWasCreated extends DomainEvent
{
    public function __construct(
        private UserId $userId,
    ) {
    }

    public function getUserId(): UserId
    {
        return $this->userId;
    }
}
```

## Record the event

Once you created the event, you need to record it somewhere. Usually this is done in an aggregate.
In this example the `User` class extends `AggregateRoot`. 
This class provides a `recordThat()` method that allows you to record events.

```php showLineNumbers title="User.php"
class User extends AggregateRoot
{
    private function __construct(
       private UserId $userId,
    ) {
    }

    public static function create(
        UserId $userId,
    ): self {
        $user = new self($userId);
        $user->recordThat(new UserWasCreated($userId));

        return $user;
    }
}
```

## Publish the event

After recording the event, it needs to be published so all event listeners can react to it if they want to.
This can be done by using the `EventBus`. 

It's possible to use the EventBus anywhere, but usually events are published right 
after the aggregate is persisted to the database. 
Just extend your repository with `DbalAggregateRootRepository` and call `$this->publishEvents()`:

```php showLineNumbers title="UserRepository.php"
class UserRepository extends DbalAggregateRootRepository
{
    public function add(User $user): void
    {
        $this->connection->insert(/*...*/);
        $this->publishEvents($user->getRecordedEvents());
    }
}
```

## Listen to the event

> An event listener is a function/method/class that handles or responds to a specific event

The final step in this process is to listen to the event. Two different types of event listeners are provided, 
one for projections in the read model and one to react to in the write model.

### ReactTo in the write model

- Add a class that extends `ConventionBasedEventListener` and add the `AsEventListener` attribute to it
- Set the event listener type to `EventListenerType::PROCESS_MANAGER`
- Add a method with the name `reactToNameOfYourEvent` that has one param that is your event

```php showLineNumbers title="UserNotificationManager.php"
#[AsEventListener(type: EventListenerType::PROCESS_MANAGER)]
class UserNotificationManager extends ConventionBasedEventListener
{
   
    public function reactToUserWasCreated(UserWasCreated $event): void
    {
        // Send out some notifications.
    }
}
```

### Project in the read model

- Add a class that extends `ConventionBasedEventListener` and add the `AsEventListener` attribute to it
- Set the event listener type to `EventListenerType::PROJECTOR`
- Add a method with the name `projectNameOfYourEvent` that has one param that is your event

```php showLineNumbers title="UserNotificationManager.php"
#[AsEventListener(type: EventListenerType::PROJECTOR)]
class UserStatsProjector extends ConventionBasedEventListener
{
   
    public function projectUserWasCreated(UserWasCreated $event): void
    {
        // Project your app's user statistics.
    }
}
```