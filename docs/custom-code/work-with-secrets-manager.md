---
id: secrets-manager
title: Working with the secrets manager
sidebar_label: Secrets manager
slug: /custom-code/secrets-manager
---

# Secrets manager

The app generated with main provider for handling with secrets - "SecretsManager".
It an injectable service that inherent from base class that work as default with the env file with the nestjs "ConfigService"

:::tip
Best practice in the industry in not to use env in production but to use external secrets manager
:::

## Example

Example of using the aws secrets manager
https://www.npmjs.com/package/nest-aws-secrets-manager

## Default SecretsManagerService

```typescript
import { Injectable } from "@nestjs/common";
import { ConfigService } from "@nestjs/config";
import { SecretsManagerServiceBase } from "./base/secretsManager.service.base";

@Injectable()
export class SecretsManagerService extends SecretsManagerServiceBase {
  constructor(protected readonly configService: ConfigService) {
    super(configService);
  }
}
```

## Customize SecretsManagerService that implament aws secret manager

```typescript
import { Injectable } from "@nestjs/common";
import { ConfigService } from "@nestjs/config";
import { SecretsRetrieverService } from "nest-aws-secrets-manager";
import { SecretsManagerServiceBase } from "./base/secretsManager.service.base";

@Injectable()
export class SecretsManagerService extends SecretsManagerServiceBase {
  constructor(
    private readonly secretsRetrieverService: SecretsRetrieverService,
    protected readonly configService: ConfigService
  ) {
    super(configService);
  }
  override async getSecret<T>(key: string): Promise<T> {
    return await this.secretsRetrieverService.getSecret<T>(key);
  }
}
```

We toke the `getSecret` function and override her with our custom code, here with the `SecretsRetrieverService` of the package `nest-aws-secrets-manager`
