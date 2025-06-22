**Summary**

Chapter 5 presents the Observer pattern, enabling objects (observers) to subscribe to events from a subject (observable). It explores Ruby’s use of callbacks, blocks, and modules to decouple event producers from consumers and demonstrates variations like pull vs. push models.

**Concepts Map**

```mermaid
flowchart LR
  subgraph Observable
    Subject[Subject (Observable)]
    Attach[add_observer]
    Notify[notify_observers]
  end
  subgraph Observer
    Obs[Observer objects]
    Update[update(state)]
  end
  Subject --> Attach
  Subject --> Notify
  Notify --> Obs
  Obs --> Update
  Pull[Pull Model] -.-> Subject
  Push[Push Model] -.-> Update
```  

**Key Concepts**

* **Observer Pattern** Defines a one-to-many dependency between objects.
* **Subject (Observable)** Maintains a list of observers and sends notifications.
* **Observer** Object that registers for and receives updates.
* **Push vs. Pull** Push sends data with notification; pull lets observers request state.
* **Callbacks & Blocks** Ruby idioms to register and handle events.
* **Separation of Concerns** Decouples event generation from handling logic.

**Quiz 20250622_14:00:00**

1. What does the Observer pattern primarily solve?
- a) Object creation
- b) Event notification
- c) Algorithm structuring
- d) Resource cleanup

2. In Ruby, which method typically registers an observer?
- a) add_listener
- b) add_observer
- c) subscribe
- d) watch

3. Push model differs from pull model by:
- a) Observers requesting state
- b) Subject sending data with notification
- c) Using Procs instead of methods
- d) Inheritance of callbacks

4. Which Ruby feature simplifies observer implementation?
- a) alias_method
- b) blocks
- c) threads
- d) regular expressions

5. The `notify_observers` call does:
- a) Clears observer list
- b) Invokes update on each observer
- c) Changes subject state
- d) Deletes subject

6. Observers should implement:
- a) a `notify` method
- b) an `update` method
- c) `handle_event`
- d) `on_change`

7. Removing an observer is done via:
- a) remove_observer
- b) unsubscribe
- c) detach
- d) forget

8. Observer pattern promotes:
- a) Tight coupling
- b) Decoupling of components
- c) Monolithic design
- d) Circular dependencies

9. A variation on Observer uses:
- a) Singleton for observers
- b) Event bus or Pub/Sub
- c) Decorator
- d) Builder

10. A drawback of Observer is:
- a) Hard to add new observers
- b) Potential memory leaks if not removed
- c) No runtime flexibility
- d) Slow object creation

**Answers:**
1. b) Event notification — manages subscriptions and updates.
2. b) add_observer — registers observer in Ruby’s standard library.
3. b) Subject sending data with notification — push includes state.
4. b) blocks — callback registration with blocks.
5. b) Invokes update on each observer — notifies all.
6. b) an `update` method — standard callback signature.
7. a) remove_observer — unregisters observer.
8. b) Decoupling of components — loose coupling between subject and observers.
9. b) Event bus or Pub/Sub — generalizes observer to messaging.
10. b) Potential memory leaks if not removed — observers lingering.

**Challenge**

Implement a simple event dispatcher using blocks: allow registering handlers for named events and triggering with payloads. Show usage registering two handlers and dispatching an event.

**Challenge Answer:**
```ruby
class EventDispatcher
  def initialize; @handlers = Hash.new { |h,k| h[k] = [] }; end
  def on(event, &block); @handlers[event] << block; end
  def trigger(event, payload=nil)
    @handlers[event].each { |h| h.call(payload) }
  end
end

d = EventDispatcher.new
# Register handlers
n=0
d.on(:data_received) { |p| puts "Handler1: #{p}" }
d.on(:data_received) { |p| puts "Handler2: #{p}" }
# Dispatch event
d.trigger(:data_received, 'payload')
```