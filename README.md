# Webhook Center

This project is a simple Ruby script to interact with webhooks. It allows you to:

- Retrieve and display information about a webhook.
- Send a message through a webhook.
- Change the name of a webhook.
This script was created for training purposes to practice Ruby programming.

## Features

- Retrieve and display webhook information.
- Send messages to the webhook.
- Change the webhook's name.
- Simple menu-driven interface.

## Usage

- Clone the repository.
- Run the script using Ruby.
- Follow the on-screen prompts to interact with the webhook.

## Prerequisites

- Ruby installed on your system. (https://rubyinstaller.org/)

## Running the Script

- Clone the repository or download the main.rb file.
- Open a terminal and navigate to the directory containing main.rb.
Run the script:
```Ruby main.rb```

-Enter the webhook URL when prompted.

## Code Explanation
main.rb

```require 'net/http'
require 'json'
require 'io/console'

def clear_screen
  system("clear") || system("cls")
end

def print_ascii_art
  cyan = "\e[36m"
  reset = "\e[0m"
  puts "#{cyan}
   ____                            _           _ 
  / ___|___  _ __  _ __   ___  ___| |_ ___  __| |
 | |   / _ \\| '_ \\| '_ \\ / _ \\/ __| __/ _ \\/ _` |
 | |__| (_) | | | | | | |  __/ (__| ||  __/ (_| |
  \\____\\___/|_| |_|_| |_|\\___|\\___|\\__\\___|\\__,_|
  #{reset}"
end

def webhookinfo(url)
  uri = URI(url)
  response = Net::HTTP.get(uri)
  JSON.parse(response)
rescue
  { "error" => "Unable to retrieve webhook information." }
end

def sendmessage(url, message)
  uri = URI(url)
  request = Net::HTTP::Post.new(uri, 'Content-Type' => 'application/json')
  request.body = { content: message }.to_json
  response = Net::HTTP.start(uri.hostname, uri.port, use_ssl: true) { |http| http.request(request) }
  response.code == '204' ? "Message sent successfully!" : "Failed to send message."
end

def webhookname(url, new_name)
  uri = URI(url)
  request = Net::HTTP::Patch.new(uri, 'Content-Type' => 'application/json')
  request.body = { name: new_name }.to_json
  response = Net::HTTP.start(uri.hostname, uri.port, use_ssl: true) { |http| http.request(request) }
  response.code == '200' ? "Webhook name changed successfully!" : "Failed to change webhook name."
end

def menu(webhook_info)
  clear_screen
  print_ascii_art
  puts "\nWebhook Information:\n\n"
  webhook_info.each do |key, value|
    puts "#{key.capitalize}: #{value}"
  end
  violet = "\e[35m"
  reset = "\e[0m"
  puts "\n#{violet}1. Send a message through the webhook#{reset}"
  puts "#{violet}2. Change the webhook name#{reset}"
  puts "#{violet}3. Exit#{reset}"
  print "\nConnected~> "
end

def main
  puts "Enter the webhook URL:"
  webhook_url = gets.chomp

  webhook_info = webhookinfo(webhook_url)
  if webhook_info['error']
    puts "Error: #{webhook_info['error']}"
    return
  end

  loop do
    menu(webhook_info)
    choice = gets.chomp.to_i

    case choice
    when 1
      print "Enter the message to send: "
      message = gets.chomp
      result = sendmessage(webhook_url, message)
      puts result
    when 2
      print "Enter the new name for the webhook: "
      new_name = gets.chomp
      result = webhookname(webhook_url, new_name)
      puts result
      webhook_info = webhookinfo(webhook_url)
    when 3
      break
    else
      puts "Invalid option. Please choose again."
    end

    puts "\nPress any key to return to the menu..."
    STDIN.getch
  end
end

main
```

This code performs the following actions:

- Clears the terminal screen.
- Prints an ASCII art header.
- Retrieves and displays webhook information.
- Sends messages to the webhook.
- Changes the webhook's name.
- Displays a menu for the user to choose an action.


Feel free to use and modify this code for your own purposes.
