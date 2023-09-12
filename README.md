Certainly, here's the step-by-step guide I provided earlier in Markdown format for your convenience:

```markdown
# Nest.js CRUD Example with Prisma, UUIDs, and PostgreSQL

In this guide, we'll create a full CRUD (Create, Read, Update, Delete) example using Nest.js, Prisma, UUIDs, and PostgreSQL. Make sure you have a Nest.js project set up and Prisma configured before starting.

## Step 1: Set Up Your Nest.js Project with Prisma

Ensure you have a Nest.js project with Prisma configured to work with PostgreSQL. Define your data models in the Prisma schema.

## Step 2: Define a Data Model with UUID

In your Prisma schema (`schema.prisma`), define a data model that includes a UUID field:

```prisma
model Post {
  id     String @id @default(auto()) @map("id", dbgenerated("uuid-ossp"))
  title  String
  content String
}
```

Ensure that the `id` field is of type `String` to store UUID values.

## Step 3: Generate Prisma Client

Generate the Prisma Client based on your schema using the following command:

```bash
npx prisma generate
```

## Step 4: Create a Service for CRUD Operations

Create a service in your Nest.js application to encapsulate CRUD operations for the `Post` model. Use the Prisma Client to interact with the database. Here's an example service:

```typescript
import { Injectable } from '@nestjs/common';
import { PrismaService } from './prisma.service';

@Injectable()
export class PostService {
  constructor(private prisma: PrismaService) {}

  async createPost(data: { title: string; content: string }) {
    return this.prisma.post.create({
      data,
    });
  }

  async getPosts() {
    return this.prisma.post.findMany();
  }

  async getPost(id: string) {
    return this.prisma.post.findUnique({
      where: {
        id,
      },
    });
  }

  async updatePost(id: string, data: { title: string; content: string }) {
    return this.prisma.post.update({
      where: {
        id,
      },
      data,
    });
  }

  async deletePost(id: string) {
    return this.prisma.post.delete({
      where: {
        id,
      },
    });
  }
}
```

## Step 5: Create a Controller

Create a controller that handles HTTP requests and interacts with the `PostService`. Define routes for creating, reading, updating, and deleting posts. Here's an example controller:

```typescript
import { Controller, Get, Post, Param, Body, Put, Delete } from '@nestjs/common';
import { PostService } from './post.service';

@Controller('posts')
export class PostController {
  constructor(private postService: PostService) {}

  @Get()
  async getPosts() {
    return this.postService.getPosts();
  }

  @Get(':id')
  async getPost(@Param('id') id: string) {
    return this.postService.getPost(id);
  }

  @Post()
  async createPost(@Body() data: { title: string; content: string }) {
    return this.postService.createPost(data);
  }

  @Put(':id')
  async updatePost(@Param('id') id: string, @Body() data: { title: string; content: string }) {
    return this.postService.updatePost(id, data);
  }

  @Delete(':id')
  async deletePost(@Param('id') id: string) {
    return this.postService.deletePost(id);
  }
}
```

## Step 6: Set Up Routes

Configure routes in your `app.module.ts` file to use the `PostController`:

```typescript
import { Module } from '@nestjs/common';
import { PostController } from './post.controller';
import { PostService } from './post.service';
import { PrismaService } from './prisma.service';

@Module({
  controllers: [PostController],
  providers: [PostService, PrismaService],
})
export class PostModule {}
```

## Step 7: Test Your API

You can now test your API by making HTTP requests to the routes defined in the `PostController`. Ensure that your PostgreSQL database is running and properly configured in your Prisma configuration.
```

Feel free to copy and paste this Markdown into a `.md` file in your project or documentation for reference.
