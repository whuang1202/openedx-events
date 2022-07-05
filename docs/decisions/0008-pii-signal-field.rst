8. PIIs inside the signal field
============================

Status
------

Provisional

Context
-------

* Consuming certain signals can sometimes contain sensitive user information that we currently process under a UserData field,
where the user's personal data is stored under the pii field. 

* Upon serialization and deserialization of the event data of this signal, the user's pii currently remains intact as the pii 
is a mandatory field that allows the serializers to work (otherwise an error is thrown which states a field doesn't exist).

* The LMS couples the pii data to events as it is the identity provider.

* Since we are dealing with user's personal data, it is a legal concern if we were to retain the user's data (especially pii) infinitely.
 
Decision
--------

- The event bus serializer/deserializer will function by keeping the pii inside the user's data intact, with potential future work 
on processing retirement events which dispose of pii data. 

Consequences
------------

- All UserData must also come with UserPersonalData, which means for every user, there is a way to access their pii.
- Data will be kept in Kafka until retirement events are processed, but since the event bus is like a notification and update propagation mechanism,
  it's highly unlikely that any old (data that has been there for a while) user pii will be accessed. 
- LMS will continue to emit the user-update event.
- Other services can emit user data (with pii) if necessary.

Deferred/Rejected Decisions
---------------------------

- Rejecting a solution of a binary configuration on whether pii messages get sent to the event bus or not. 

