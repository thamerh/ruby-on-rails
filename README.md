# Complete Guide to Learning Ruby on Rails

## Basic Ruby Syntax and Variable Declaration

```ruby
# Variable declaration
variable_name = "value"
number_variable = 10
array_variable = [1, 2, 3]
hash_variable = { key: "value", another_key: "another value" }

# Conditional statement
if condition
  # code to execute if condition is true
else
  # code to execute if condition is false
end

# Looping (example of each loop)
array_variable.each do |element|
  puts element
end

# Method definition
def method_name(parameter1, parameter2)
  # method body
end

# Class definition
class ClassName
  def initialize(parameter)
    @parameter = parameter
  end

  def instance_method
    # method body
  end
end
```

## Installing Ruby on Rails

```bash
# Install Bundler
$ gem install bundler

# Install Rails
$ gem install rails

# Install all required gems specified in the Gemfile
$ bundle install
```

## Creating a New Rails Application

```bash
# Create a new Rails application configured for API development
$ rails new Example -d postgresql --api
$ cd Example
```

## Configuring the Database

```yaml
# PostgreSQL configuration for Rails
default: &default
  adapter: postgresql
  username: username
  password: password
  host: localhost
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  timeout: 5000

development:
  <<: *default
  database: example_development

test:
  <<: *default
  database: example_test

production:
  <<: *default
  database: example_production
```

## Generating Models and Migrating the Database

```bash
# Create a model
$ rails g model ModelName key:type key1:type1 key2:type2

# Create the database
$ rails db:create

# Run migrations
$ rails db:migrate
```

## Generating Controllers

```bash
# Create a controller (auto-generate controller name)
$ rails g controller api/v1/ControllerName
```

## Defining Routes

```ruby
Rails.application.routes.draw do
  namespace :api do
    namespace :v1 do
      resources :controller_name
    end
  end
end
```

## Creating Controller Actions

```ruby
class Api::V1::ControllerNameController < ApplicationController
  before_action :set_model_name, only: [:show, :update, :destroy]

  def index
    @data = ModelName.all
    render json: @data
  end

  def show
    render json: @model_name
  end

  def create
    @model_instance = ModelName.new(model_name_params)
    if @model_instance.save
      render json: @model_instance, status: :created
    else
      render json: @model_instance.errors, status: :unprocessable_entity
    end
  end

  def update
    if @model_name.update(model_name_params)
      render json: @model_name
    else
      render json: @model_name.errors, status: :unprocessable_entity
    end
  end

  def destroy
    @model_name.destroy
    head :no_content
  end

  private

  def set_model_name
    @model_name = ModelName.find_by(id: params[:id])
    render json: { error: 'ModelName not found' }, status: :not_found unless @model_name
  end

  def model_name_params
    params.require(:model_name).permit(:attribute1, :attribute2)
  end
end
```

## Seeding the Database

```ruby
# db/seeds.rb
ModelName.create!(key1: "example", value1: "example")
ModelName.create!(key2: "example", value2: "example")
ModelName.create!(key3: "example", value3: "example")
```

```bash
# Run the seed to populate the database
$ rails db:seed
```

## Testing the API

### Making Requests

- **GET**: Set the request type to "GET" and enter your API endpoint URL:
  `http://localhost:port/api/v1/controller_name`

- **POST**: Set the request type to "POST" and use the same API endpoint URL. In the "Body" tab, select "raw," and choose "JSON (application/json)" from the dropdown.

- **PUT**: Set the request type to "PUT" and use the endpoint URL with the record's ID:
  `http://localhost:port/api/v1/controller_name/:id` (replace `:id` with the actual record ID). In the "Body" tab, choose "raw" and "JSON (application/json)".

- **DELETE**: Set the request type to "DELETE" and use the endpoint URL with the record's ID:
  `http://localhost:port/api/v1/controller_name/:id`

## Connecting Frontend to Backend

```ruby
# Gemfile
gem 'rack-cors'
```

```bash
# Install the gem via the terminal
$ bundle install
```

```ruby
# config/initializers/cors.rb
Rails.application.config.middleware.insert_before 0, Rack::Cors do
  allow do
    origins "http://localhost:PORT"

    resource "*",
      headers: :any,
      methods: [:get, :post, :put, :patch, :delete, :options, :head]
  end
end
```

This Markdown file provides a complete syntax-highlighted guide to setting up and using Ruby on Rails for API development, covering installation, database configuration, model and controller generation, routing, seeding, API testing, configuring CORS for frontend connectivity, and includes basic Ruby syntax and variable declarations for beginners.
```