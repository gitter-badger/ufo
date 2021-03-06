---
title: ufo tasks build
---

The command `ufo tasks build` generates the task definitions locally and writes them to the `output/` folder.  There are 2 files that it uses in order to produce the raw AWS task definitions files.

1. ufo/templates/main.json.erb
2. ufo/task_definitions.rb

Here's an example of each of them:

**main.json.erb**:

```json
{
    "family": "<%= @family %>",
    "containerDefinitions": [
        {
            "name": "<%= @name %>",
            "image": "<%= @image %>",
            "cpu": <%= @cpu %>,
            <% if @memory %>
            "memory": <%= @memory %>,
            <% end %>
            <% if @memory_reservation %>
            "memoryReservation": <%= @memory_reservation %>,
            <% end %>
            <% if @container_port %>
            "portMappings": [
                {
                    "containerPort": "<%= @container_port %>",
                    "protocol": "tcp"
                }
            ],
            <% end %>
            "command": <%= @command.to_json %>,
            <% if @environment %>
            "environment": <%= @environment.to_json %>,
            <% end %>
            <% if @awslogs_group %>
            "logConfiguration": {
                "logDriver": "awslogs",
                "options": {
                    "awslogs-group": "<%= @awslogs_group %>",
                    "awslogs-region": "<%= @awslogs_region || 'us-east-1' %>",
                    "awslogs-stream-prefix": "<%= @awslogs_stream_prefix %>"
                }
            },
            <% end %>
            "essential": true
        }
    ]
}
```

**task_definitions.rb**:

```
# common variables
common = {
  image: helper.full_image_name, # includes the git sha tongueroo/hi:ufo-[sha].
  cpu: 128,
  memory_reservation: 256,
  environment: helper.env_file(".env")
  # another example
  # environment: helper.env_vars(%Q{
  #   RAILS_ENV=production
  #   SECRET_KEY_BASE=secret
  # })
}

task_definition "hi-web-stag" do
  source "main" # will use ufo/templates/main.json.erb
  variables(common.dup.deep_merge(
    family: task_definition_name,
    name: "web",
    container_port: helper.dockerfile_port,
    command: ["bin/web"]
  ))
end

task_definition "hi-worker-stag" do
  source "main" # will use ufo/templates/main.json.erb
  variables(common.dup.deep_merge(
    family: task_definition_name,
    name: "worker",
    command: ["bin/worker"]
  ))
end

task_definition "hi-clock-stag" do
  source "main" # will use ufo/templates/main.json.erb
  variables(common.dup.deep_merge(
    family: task_definition_name,
    name: "clock",
    command: ["bin/clock"]
  ))
end
```

Ufo uses the ERB template in `main.json.erb` and combines it with the variables defined in the task definition declarations in `task_definitions.rb` and generates the raw AWS formatted task definition in the `output` folder.  In this case there are 3 task_definitions so there will be 3 generated output files.

To build the task definitions:

```sh
ufo tasks build
```

You should see output similar to below:

<img src="/img/tutorials/ufo-tasks-build.png" class="doc-photo" />

Let's take a look at one of the generated files: `ufo/output/hi-web-stag.json`.

```json
{
  "family": "hi-web-stag",
  "containerDefinitions": [
    {
      "name": "web",
      "image": "tongueroo/hi:ufo-2017-06-11T22-22-32-a18aa30",
      "cpu": 128,
      "memoryReservation": 256,
      "portMappings": [
        {
          "containerPort": "3000",
          "protocol": "tcp"
        }
      ],
      "command": [
        "bin/web"
      ],
      "environment": [
        {
          "name": "RAILS_ENV",
          "value": "staging"
        }
      ],
      "essential": true
    }
  ]
}
```

If you need to modify the task definition template to suite your own needs it is super simple, just edit `main.json.erb`.  No need to dive deep into internal code that builds up the task definition with some internal structure.  It is all there for you to fully control.

<a id="prev" class="btn btn-basic" href="{% link _docs/ufo-docker-clean.md %}">Back</a>
<a id="next" class="btn btn-primary" href="{% link _docs/ufo-tasks-register.md %}">Next Step</a>
<p class="keyboard-tip">Pro tip: Use the <- and -> arrow keys to move back and forward.</p>

