# Introduction 

In Ruby on Rails, Single Table Inheritance (STI) is a technique used to model inheritance relationships between ActiveRecord models. With STI, multiple models share a single database table, and a type column is used to differentiate between the different types of records in that table. This allows you to have a single table that contains data for multiple related models, with each model inheriting behavior and attributes from a common superclass.

# Implementation 

Let's say we have a `Vehicle` model with subclasses `Car` and `Truck`. We can set up the `Vehicle` model as follows:

```ruby
# app/models/vehicle.rb
class Vehicle < ApplicationRecord
end
```

And then define the `Car` and `Truck` models as subclasses of `Vehicle`:

```ruby
# app/models/car.rb
class Car < Vehicle
end

# app/models/truck.rb
class Truck < Vehicle
end
```

In your database schema, you would only need one table, let's call it `vehicles`, which would include a `type` column to indicate the specific type of vehicle:

```ruby
class CreateVehicles < ActiveRecord::Migration[6.0]
  def change
    create_table :vehicles do |t|
      t.string :type
      t.string :make
      t.string :model
      # Add other common attributes for vehicles here
      t.timestamps
    end
  end
end
```

Now, when you create a new `Car` or `Truck` record, Rails will automatically populate the `type` column with the appropriate class name. For example:

```ruby
Car.create(make: 'Toyota', model: 'Camry')
Truck.create(make: 'Ford', model: 'F-150')
```

This will result in entries in the `vehicles` table with `type` column set to `'Car'` or `'Truck'` respectively, allowing you to distinguish between the two types of vehicles while still storing them in the same table.
