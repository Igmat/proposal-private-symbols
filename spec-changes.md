# ECMA262 Specification Changes

## Changes to the Symbol Type

### 6.1.5 The Symbol Type

Add the following item to the description of the Symbol type:

- Each Symbol value immutably holds a value called [[Private]] that is either **true** or **false**.

### 6.1.5.1 Well-Known Symbols

Add a clause specifying that well-known symbols have a [[Private]] value of **false**.

## Changes to OwnPropertyKeys

### 6.1.7.3 Invariants of the Essential Internal Methods

#### [[OwnPropertyKeys]] ( )

Modify the list of invariants:

- The return value must be a List.
- The returned list must not contain any duplicate entries.
- The Type of each element of the returned List is either String or Symbol.
- The returned list must not contain any symbols whose [[Private]] value is **true**.
- The returned list must contain at least the keys of all non-configurable non-private own properties that have previously been observed.
- If the object is non-extensible, the returned list must contain only the keys of all non-private own properties of the object that are observable using [[GetOwnProperty]].

*The term "non-private own property" is not well-defined.*

### 9.1.11.1 OrdinaryOwnPropertyKeys ( _O_ )

Modify step 4:

- For each own property key _P_ of _O_ that is a Symbol, in ascending chronological order of property creation, do
  - If _P_'s [[Private]] value is **false**, add _P_ as the last element of _keys_.

Update identical steps for exotic object [[OwnPropertyKeys]]:

- 9.4.3.3 (String)
- 9.4.5.6 (Integer-Indexed)

## Changes to Proxy Internal Methods

### 9.5.5 [[GetOwnProperty]] ( _P_ )

Add after step 1:

- If Type(_P_) is Symbol and  _P_'s [[Private]] value is **true**, throw a TypeError exception.

Update similar steps for the following internal methods:

- 9.5.6 [[DefineOwnProperty]] ( P, Desc )
- 9.5.7 [[HasProperty]] ( P )
- 9.5.8 [[Get]] ( P, Receiver )
- 9.5.9 [[Set]] ( P, V, Receiver )
- 9.5.10 [[Delete]] ( P )

### 9.5.11 [[OwnPropertyKeys]] ( )

Add after step 9:

- If _trapResult_ contains any Symbol values whose [[Private]] value is **true**, throw a **TypeError** exception.

Add the following item to the note:

- The returned List contains no Symbol values whose [[Private]] value is **true**.

Modify note regarding invariants:

- The result of [[OwnPropertyKeys]] is a List.
- The returned List contains no duplicate entries.
- The Type of each result List element is either String or Symbol.
- The returned list must not contain any symbols whose [[Private]] value is **true**.
- The result List must contain the keys of all non-configurable non-private own properties of the target object.
- If the target object is not extensible, then the result List must contain all the keys of the non-private own properties of the target object and no other values.

## Changes to the Symbol API

### 19.4.1.1 Symbol ( [ _description_ ] )

Modify step 4:

- Return a new unique Symbol value whose [[Description]] value is _descString_ and whose [[Private]] value is **false**.
