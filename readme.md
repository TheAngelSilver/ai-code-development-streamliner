AI Code Development Streamliner

![License](https://img.shields.io/github/license/TheAngelSilver/ai-code-development-streamliner)
![PHP](https://img.shields.io/badge/PHP-8.1%2B-blue)
![Composer](https://img.shields.io/badge/Composer-2.0%2B-blue)

## Table of Contents

- [Introduction](#introduction)
- [Features](#features)
- [Technology Stack](#technology-stack)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Configuration](#configuration)
- [Database Setup](#database-setup)
- [API Integration](#api-integration)
- [Context Management](#context-management)
- [Integration with VSCode Cody Extension](#integration-with-vscode-cody-extension)
- [Security Considerations](#security-considerations)
- [Testing](#testing)
- [Deployment](#deployment)
- [Contributing](#contributing)
- [License](#license)
- [Contact](#contact)

## Introduction

**AI Code Development Streamliner** is a PHP-based web application designed to enhance and streamline AI-assisted code development. By leveraging the OpenAI API, this tool organizes project histories, maintains contextual information, and integrates seamlessly with the VSCode Cody extension to provide a cohesive and efficient development workflow.

## Features

- **OpenAI API Integration:** Utilize OpenAI's powerful language models to generate code suggestions, completions, and more.
- **Project Organization:** Maintain individual histories and contextual data for each project.
- **Context Retrieval:** Automatically pull relevant context when resuming work on a project.
- **VSCode Integration:** Work seamlessly with the VSCode Cody extension to enhance your coding environment.
- **User-Friendly Interface:** Manage projects, view histories, and configure settings through an intuitive web interface.
- **Secure Authentication:** Protect your projects and data with robust authentication mechanisms.
- **Extensible Architecture:** Easily extend and customize the application to fit your specific needs.

## Technology Stack

- **Backend:** PHP 8.1+
- **Dependency Management:** Composer
- **HTTP Client:** GuzzleHTTP
- **Environment Variables:** vlucas/phpdotenv
- **Database:** MySQL 8.x or PostgreSQL 14.x
- **Version Control:** Git
- **Deployment:** Apache/Nginx, Docker (optional)

## Prerequisites

Before you begin, ensure you have met the following requirements:

- **PHP:** Version 8.1 or higher
- **Composer:** Dependency management for PHP
- **Web Server:** Apache or Nginx
- **Database Server:** MySQL or PostgreSQL
- **Git:** Version control system
- **VSCode:** Installed with the Cody extension
- **OpenAI API Key:** Obtain from [OpenAI](https://beta.openai.com/signup/)

## Installation

### 1. Clone the Repository

```bash
git clone https://github.com/TheAngelSilver/ai-code-development-streamliner.git
cd ai-code-development-streamliner
2. Install Dependencies
Use Composer to install PHP dependencies:

bash
Copy code
composer install
3. Set Up Environment Variables
Copy the example environment file and configure the necessary variables:

bash
Copy code
cp .env.example .env
Edit the .env file to include your database credentials and OpenAI API key:

env
Copy code
APP_NAME=AI Code Development Streamliner
APP_ENV=local
APP_KEY=your_generated_key
APP_DEBUG=true
APP_URL=http://localhost

# Database Configuration
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=your_database
DB_USERNAME=your_username
DB_PASSWORD=your_password

# OpenAI API Key
OPENAI_API_KEY=your_openai_api_key
Generate an Application Key:

You can generate a random application key using the following PHP script:

php
Copy code
<?php
echo bin2hex(random_bytes(16));
Run it in your terminal:

bash
Copy code
php -r "echo bin2hex(random_bytes(16));"
Copy the output and set it as the value for APP_KEY in your .env file.

4. Set Up the Database
Create a New Database:

Create a new database in your preferred DBMS (e.g., MySQL).
Run Migrations:

Execute the SQL scripts provided in the database/migrations/ directory to set up the database schema.
bash
Copy code
php migrate.php
(Assuming you have a migrate.php script to handle migrations. If not, you can manually run the SQL scripts.)

(Optional) Seed the Database:

Populate the database with initial data.
bash
Copy code
php seed.php
(Assuming you have a seed.php script. Otherwise, manually insert necessary data.)

5. Configure Web Server
For Apache:
Create a Virtual Host Configuration:

apache
Copy code
<VirtualHost *:80>
    ServerName yourdomain.com
    DocumentRoot "C:/xampp_8.1/htdocs/Ai-Code-Dev_STRM/public"

    <Directory "C:/xampp_8.1/htdocs/Ai-Code-Dev_STRM/public">
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog "logs/ai-code-error.log"
    CustomLog "logs/ai-code-access.log" common
</VirtualHost>
Enable the Site and Restart Apache:

Add the virtual host to your Apache configuration.
Restart Apache to apply changes.
For Nginx:
Create a Server Block Configuration:

nginx
Copy code
server {
    listen 80;
    server_name yourdomain.com;
    root /path-to-project/public;

    index index.php index.html index.htm;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }

    location ~ /\.ht {
        deny all;
    }
}
Enable the Site and Restart Nginx:

bash
Copy code
sudo ln -s /etc/nginx/sites-available/yourdomain.com /etc/nginx/sites-enabled/
sudo systemctl restart nginx
6. (Optional) Docker Setup
Alternatively, you can use Docker to containerize the application. Refer to the docker/ directory for Dockerfile and docker-compose.yml configurations.

Configuration
Environment Variables
Ensure all necessary environment variables are set in the .env file:

APP_NAME: Name of your application.
APP_ENV: Application environment (local, production, etc.).
APP_KEY: Generated application key.
APP_DEBUG: Enable or disable debug mode (true, false).
APP_URL: Base URL of your application.
DB_CONNECTION, DB_HOST, DB_PORT, DB_DATABASE, DB_USERNAME, DB_PASSWORD: Database configuration.
OPENAI_API_KEY: Your OpenAI API key.
OpenAI API Configuration
The application uses the OpenAI API to generate code suggestions and manage contexts. Ensure the OPENAI_API_KEY is correctly set in your .env file.

Database Setup
Migrations
If you have migration scripts, execute them to set up the database schema. Here's an example migrate.php script:

php
Copy code
<?php
require 'vendor/autoload.php';

use Dotenv\Dotenv;

$dotenv = Dotenv::createImmutable(__DIR__);
$dotenv->load();

$dsn = "{$_ENV['DB_CONNECTION']}:host={$_ENV['DB_HOST']};dbname={$_ENV['DB_DATABASE']}";
$username = $_ENV['DB_USERNAME'];
$password = $_ENV['DB_PASSWORD'];

try {
    $pdo = new PDO($dsn, $username, $password);
    $pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);

    // Example migration: Create users table
    $pdo->exec("
        CREATE TABLE IF NOT EXISTS users (
            id INT AUTO_INCREMENT PRIMARY KEY,
            username VARCHAR(50) NOT NULL UNIQUE,
            email VARCHAR(100) NOT NULL UNIQUE,
            password VARCHAR(255) NOT NULL,
            created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
        )
    ");

    // Add additional migrations as needed

    echo "Migrations ran successfully.\n";
} catch (PDOException $e) {
    echo "Migration failed: " . $e->getMessage() . "\n";
    exit(1);
}
Run the migration script:

bash
Copy code
php migrate.php
Models
Define your PHP classes to interact with the database. For example, a simple User model using PDO:

php
Copy code
<?php
// src/Models/User.php

namespace Models;

use PDO;

class User
{
    private $pdo;

    public function __construct(PDO $pdo)
    {
        $this->pdo = $pdo;
    }

    public function create($username, $email, $password)
    {
        $stmt = $this->pdo->prepare("INSERT INTO users (username, email, password) VALUES (:username, :email, :password)");
        return $stmt->execute([
            ':username' => $username,
            ':email' => $email,
            ':password' => password_hash($password, PASSWORD_BCRYPT),
        ]);
    }

    // Add additional methods as needed
}
API Integration
OpenAI API
The application interacts with OpenAI's API to generate code suggestions and manage context. Here's how it's integrated using GuzzleHTTP:

Install GuzzleHTTP via Composer:

bash
Copy code
composer require guzzlehttp/guzzle
Create an API Client:

php
Copy code
<?php
// src/Services/OpenAIService.php

namespace Services;

use GuzzleHttp\Client;

class OpenAIService
{
    private $client;
    private $apiKey;

    public function __construct()
    {
        $this->apiKey = getenv('OPENAI_API_KEY');
        $this->client = new Client([
            'base_uri' => 'https://api.openai.com/v1/',
            'headers' => [
                'Authorization' => 'Bearer ' . $this->apiKey,
                'Content-Type' => 'application/json',
            ],
        ]);
    }

    public function generateCode($prompt, $maxTokens = 150, $temperature = 0.7)
    {
        $response = $this->client->post('completions', [
            'json' => [
                'model' => 'text-davinci-003',
                'prompt' => $prompt,
                'max_tokens' => $maxTokens,
                'temperature' => $temperature,
            ],
        ]);

        return json_decode($response->getBody(), true);
    }

    // Add additional methods as needed
}
Usage Example:

php
Copy code
<?php
// public/generate.php

require '../vendor/autoload.php';

use Services\OpenAIService;

$dotenv = Dotenv\Dotenv::createImmutable(__DIR__ . '/../');
$dotenv->load();

$openAI = new OpenAIService();

$prompt = "Create a PHP function to connect to a MySQL database.";
$result = $openAI->generateCode($prompt);

echo "<pre>";
print_r($result);
echo "</pre>";
Access this script via http://yourdomain.com/generate.php to see the generated code.

Context Management
Storing Context
Contextual information for each project is stored in the database. Create a contexts table to hold this data.

Migration Example:

php
Copy code
<?php
// src/migrations/create_contexts_table.php

use PDO;

require 'vendor/autoload.php';

$dotenv = Dotenv\Dotenv::createImmutable(__DIR__ . '/../');
$dotenv->load();

$dsn = "{$_ENV['DB_CONNECTION']}:host={$_ENV['DB_HOST']};dbname={$_ENV['DB_DATABASE']}";
$username = $_ENV['DB_USERNAME'];
$password = $_ENV['DB_PASSWORD'];

try {
    $pdo = new PDO($dsn, $username, $password);
    $pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);

    $pdo->exec("
        CREATE TABLE IF NOT EXISTS contexts (
            id INT AUTO_INCREMENT PRIMARY KEY,
            project_id INT NOT NULL,
            context TEXT NOT NULL,
            created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
            FOREIGN KEY (project_id) REFERENCES projects(id) ON DELETE CASCADE
        )
    ");

    echo "Contexts table created successfully.\n";
} catch (PDOException $e) {
    echo "Migration failed: " . $e->getMessage() . "\n";
    exit(1);
}
Run the migration:

bash
Copy code
php src/migrations/create_contexts_table.php
Retrieving Context
Fetch the latest context for a project and include it in API requests.

Example:

php
Copy code
<?php
// src/Services/ContextService.php

namespace Services;

use PDO;

class ContextService
{
    private $pdo;

    public function __construct(PDO $pdo)
    {
        $this->pdo = $pdo;
    }

    public function getLatestContext($projectId)
    {
        $stmt = $this->pdo->prepare("SELECT context FROM contexts WHERE project_id = :project_id ORDER BY created_at DESC LIMIT 1");
        $stmt->execute([':project_id' => $projectId]);
        return $stmt->fetchColumn();
    }

    public function addContext($projectId, $context)
    {
        $stmt = $this->pdo->prepare("INSERT INTO contexts (project_id, context) VALUES (:project_id, :context)");
        return $stmt->execute([
            ':project_id' => $projectId,
            ':context' => $context,
        ]);
    }

    // Add additional methods as needed
}
Integration with VSCode Cody Extension
Overview
The VSCode Cody extension provides AI-assisted coding features within the VSCode environment. This application complements Cody by managing project contexts and histories.

Steps to Integrate
Expose API Endpoints:

Ensure the application has API endpoints to fetch and update context. Refer to the API Documentation for details.
Authentication:

Implement secure authentication (e.g., API tokens) for communication between Cody and the application.
Custom Scripts:

Develop scripts or extensions that allow Cody to retrieve context from the application when needed.
Webhooks:

Set up webhooks to notify the application of changes in VSCode, enabling real-time context updates.
Example Integration Workflow
User Initiates Code Assistance:

Cody requests context for the current project from the application via API.
Application Provides Context:

The application retrieves the relevant context and sends it to Cody.
Cody Generates Suggestions:

Using the provided context, Cody generates code suggestions or completions.
User Commits Changes:

Upon committing changes, Cody sends updated context back to the application to maintain history.
Security Considerations
Protecting API Keys
Environment Variables: Store API keys and sensitive information in the .env file, which should never be committed to version control.
Server Configuration: Ensure server permissions restrict access to environment files.
Input Validation
Sanitize Inputs: Validate and sanitize all user inputs to prevent SQL injection, XSS, and other vulnerabilities.
Use Prepared Statements: When interacting with the database, use prepared statements to handle queries securely.
HTTPS
SSL Certificates: Use HTTPS to encrypt data in transit. Obtain SSL certificates from trusted authorities like Let's Encrypt.
Force HTTPS: Configure the web server to redirect all HTTP traffic to HTTPS.
Authentication & Authorization
User Authentication: Implement robust authentication mechanisms (e.g., token-based authentication).
Role-Based Access Control: Define user roles and permissions to restrict access to sensitive features and data.
Testing
Unit Testing
Write unit tests for individual components using PHPUnit.

Install PHPUnit via Composer:

bash
Copy code
composer require --dev phpunit/phpunit
Create a Test Class:

php
Copy code
<?php
// tests/Services/OpenAIServiceTest.php

use PHPUnit\Framework\TestCase;
use Services\OpenAIService;

class OpenAIServiceTest extends TestCase
{
    public function testGenerateCode()
    {
        $openAI = new OpenAIService();
        $prompt = "Create a PHP function to connect to a MySQL database.";
        $result = $openAI->generateCode($prompt);

        $this->assertArrayHasKey('choices', $result);
        $this->assertNotEmpty($result['choices']);
        $this->assertArrayHasKey('text', $result['choices'][0]);
    }
}
Run Tests:

bash
Copy code
./vendor/bin/phpunit --bootstrap vendor/autoload.php tests
Integration Testing
Ensure that different parts of the application work together as expected.

Example: Testing Database Interaction

php
Copy code
<?php
// tests/Services/ContextServiceTest.php

use PHPUnit\Framework\TestCase;
use Services\ContextService;

class ContextServiceTest extends TestCase
{
    private $pdo;
    private $contextService;

    protected function setUp(): void
    {
        $dsn = "{$_ENV['DB_CONNECTION']}:host={$_ENV['DB_HOST']};dbname={$_ENV['DB_DATABASE']}";
        $username = $_ENV['DB_USERNAME'];
        $password = $_ENV['DB_PASSWORD'];

        $this->pdo = new PDO($dsn, $username, $password);
        $this->pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);

        $this->contextService = new ContextService($this->pdo);
    }

    public function testAddAndRetrieveContext()
    {
        $projectId = 1;
        $context = "Initial project context.";

        $this->contextService->addContext($projectId, $context);
        $retrievedContext = $this->contextService->getLatestContext($projectId);

        $this->assertEquals($context, $retrievedContext);
    }
}
Mocking External Services
Use mocking to simulate external API responses.

Example: Mocking OpenAI API Response

php
Copy code
<?php
// tests/Services/OpenAIServiceMockTest.php

use PHPUnit\Framework\TestCase;
use Services\OpenAIService;
use GuzzleHttp\Client;
use GuzzleHttp\Psr7\Response;
use GuzzleHttp\Handler\MockHandler;
use GuzzleHttp\HandlerStack;

class OpenAIServiceMockTest extends TestCase
{
    public function testGenerateCodeWithMock()
    {
        // Create a mock and queue a response.
        $mock = new MockHandler([
            new Response(200, [], json_encode([
                'choices' => [
                    ['text' => 'Mocked PHP function to connect to MySQL.']
                ]
            ])),
        ]);

        $handlerStack = HandlerStack::create($mock);
        $client = new Client(['handler' => $handlerStack]);

        // Inject the mock client into OpenAIService
        $openAI = new OpenAIService();
        $openAI->setClient($client); // Assuming you have a setter method

        $prompt = "Create a PHP function to connect to a MySQL database.";
        $result = $openAI->generateCode($prompt);

        $this->assertArrayHasKey('choices', $result);
        $this->assertEquals('Mocked PHP function to connect to MySQL.', $result['choices'][0]['text']);
    }
}
Continuous Integration
Integrate testing into your CI/CD pipeline using GitHub Actions.

Example .github/workflows/ci.yml:

yaml
Copy code
name: CI

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main, develop ]

jobs:
  build:

    runs-on: ubuntu-latest

    services:
      mysql:
        image: mysql:5.7
        env:
          MYSQL_ROOT_PASSWORD: root
          MYSQL_DATABASE: ai_dev_streamliner
          MYSQL_USER: db_user
          MYSQL_PASSWORD: securepassword
        ports:
          - 3306:3306
        options: >-
          --health-cmd="mysqladmin ping --silent"
          --health-interval=10s
          --health-timeout=5s
          --health-retries=3

    steps:
    - uses: actions/checkout@v2

    - name: Set up PHP
      uses: shivammathur/setup-php@v2
      with:
        php-version: '8.1'
        extensions: mbstring, bcmath, pdo_mysql
        coverage: xdebug

    - name: Install Dependencies
      run: |
        composer install --prefer-dist --no-progress --no-suggest

    - name: Copy .env
      run: cp .env.example .env

    - name: Generate key
      run: php -r "file_put_contents('.env', str_replace('APP_KEY=', 'APP_KEY=' . bin2hex(random_bytes(16)) . "\n", file_get_contents('.env')));"

    - name: Wait for MySQL
      run: |
        for i in {1..30}; do
          mysqladmin ping -h localhost --password=root --silent && break
          echo "Waiting for MySQL..."
          sleep 1
        done

    - name: Run Migrations
      run: php migrate.php

    - name: Run Tests
      run: ./vendor/bin/phpunit --bootstrap vendor/autoload.php tests
Deployment
Preparing for Deployment
Set Environment Variables:

Set APP_ENV=production and APP_DEBUG=false in the .env file.
Optimize Autoloader:

bash
Copy code
composer install --optimize-autoloader --no-dev
Server Setup
Web Server Configuration:

Ensure Apache or Nginx is properly configured to serve the application.
File Permissions:

bash
Copy code
sudo chown -R www-data:www-data /path-to-project/storage
sudo chown -R www-data:www-data /path-to-project/public
Deploying Code
Clone the Repository on Server:

bash
Copy code
git clone https://github.com/TheAngelSilver/ai-code-development-streamliner.git
cd ai-code-development-streamliner
Install Dependencies:

bash
Copy code
composer install --optimize-autoloader --no-dev
Set Up Environment Variables:

bash
Copy code
cp .env.example .env
Update the .env file with production settings.
Generate Application Key:

bash
Copy code
php -r "file_put_contents('.env', str_replace('APP_KEY=', 'APP_KEY=' . bin2hex(random_bytes(16)) . "\n", file_get_contents('.env')));"
Run Migrations:

bash
Copy code
php migrate.php
Optimize the Application:

bash
Copy code
# No specific optimization commands for pure PHP, but you can use OPCache
Monitoring and Logging
Logs: Monitor application logs located in the logs/ directory.
Monitoring Tools: Use tools like New Relic or Sentry for real-time monitoring and error tracking.
Contributing
Contributions are welcome! Please follow these guidelines:

1. Fork the Repository
Fork the repository to your GitHub account.

2. Clone Your Fork
bash
Copy code
git clone https://github.com/yourusername/ai-code-development-streamliner.git
cd ai-code-development-streamliner
3. Create a Branch
bash
Copy code
git checkout -b feature/YourFeatureName
4. Make Your Changes
Implement your feature or bug fix.

5. Commit Your Changes
bash
Copy code
git add .
git commit -m "Add Your Feature"
6. Push to the Branch
bash
Copy code
git push origin feature/YourFeatureName
7. Open a Pull Request
Navigate to the original repository and click "New Pull Request."

Guidelines
Code Style: Follow the PSR-12 coding standards for PHP.
Documentation: Update documentation and README as needed.
Testing: Write tests for new features and ensure existing tests pass.
Issue Tracking: Address issues reported in the repository and reference them in your pull requests.
License
This project is licensed under the MIT License.

Contact
For any questions or suggestions, feel free to reach out:

Email: your.email@example.com
GitHub: TheAngelSilver
LinkedIn: Your Name
Thank you for using AI Code Development Streamliner! We hope this tool enhances your coding workflow and boosts your productivity.
```
