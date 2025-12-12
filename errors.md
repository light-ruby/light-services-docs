# Handling Errors in Light Services

Errors are a natural part of every application. In this guide, we'll explore how to handle errors within Light Services, drawing parallels to ActiveModel errors.

## Error Structure

Light Service errors follow a structure similar to ActiveModel errors. Here's a simplified example:

```ruby
{
  email: ["must be a valid email"],
  password: ["is too short", "must contain at least one number"]
}
```

## Adding Errors

To add an error to your service, use the `errors.add` method.

{% hint style="info" %}
By default, adding an error marks the service as failed, preventing subsequent steps from executing. This behavior can be customized in the configuration for individual services and errors.
{% endhint %}

```ruby
class ParsePage < ApplicationService
  # Arguments
  arg :url, type: :string
  # ...

  # Steps
  step :validate
  step :parse
  # ...

  private

  def validate
    # Multiple errors can be added with the same key
    errors.add(:url, "must be a valid URL") unless url.match?(URI::DEFAULT_PARSER.make_regexp)
    errors.add(:url, "must be a secure link") unless url.start_with?("https")
  end

  # ...
end
```

## Reading Errors

To check if a service has errors, you can use the `#failed?` method. You can also use methods like `errors.any?` to inspect errors.

```ruby
class ParsePage < ApplicationService
  def parse
    nodes.each do |node|
      if node.blank?
        errors.add(:base, "Node #{node} is blank")
      else
        parse_node(node)
      end
    end

    if failed? # or errors.any?
      puts "Not all nodes were parsed"
    end
  end
end
```

You can access errors outside the service using the `#errors` method.

```ruby
service = ParsePage.run(url: "rubygems")

if service.failed?
  puts service.errors
  puts service.errors[:url]
  puts service.errors.full_messages # This feature is in development and will be available soon
end
```

## Adding Warnings

Sometimes, you may want to add a warning instead of an error. Warnings are similar to errors but they do not mark the service as failed, do not stop execution, and do not roll back the transaction.

```ruby
class ParsePage < ApplicationService
  def validate
    errors.add(:url, "must be a valid URL") unless url.match?(URI::DEFAULT_PARSER.make_regexp)
    warnings.add(:url, "should be a secure link") unless url.start_with?("https")
  end
end
```

```ruby
service = ParsePage.run(url: "http://rubygems.org")

if service.warnings.any?
  puts service.warnings
  puts service.warnings[:url]
  puts service.warnings.full_messages # This feature is in development and will be available soon
end
```

By following these guidelines, you can effectively manage errors and warnings in Light Services, ensuring a smoother and more robust application experience.

## What's next?

Congratulations! Boring stuff is done. Now you can learn some best practices and start using Light Service in your projects.

[Next: Best Practices](best-practices.md)

