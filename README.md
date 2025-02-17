<p align="center">
  <img width="250" height="250" src="https://user-images.githubusercontent.com/15312980/174908438-b6f5eaea-7b81-4008-9cad-8a7c2a45bbaf.svg">
</p>

# monitor

Utilize a declarative API to wrap your functions to capture Prometheus metrics & logs for each function

## Install
```
yarn add @osskit/monitor
```

## Scoped
```
import { createMonitor } from '@osskit/monitor'

export const monitor = createMonitor({ scope: 'metrics' });

const result1 = await monitor('query', async () => db.query());
const result2 = await monitor('update', async () => db.update());

// `metrics.query`, `metrics.update` for logs & metrics
```
## Unscoped
```
import monitor from '@osskit/monitor'

const result = await monitor('query', async () => db.query());

// `query` for logs & metrics
```
## Monitor Options
```
import { createMonitor } from '@osskit/monitor'

export const monitor = createMonitor({ scope: 'metrics' });

// Context
const result = (id: string) => await monitor('query', async () => db.query(id), { context: { id } });

// Parse & Log Results
const logResults = (id: string) => await monitor('query', async () => db.query(id), { logResult: true, parseResult: (res) => res.prop });

// Parse Error
const errored = (id: string) => await monitor('query', async () => db.query(id), { logResult: true, parseError: (e) => e.statusCode });

// Log Execution Start
const executionStart = (id: string) => await monitor('query', async () => db.query(id), { logExecutionStart: true });
```

## Global Options
```
import { setGlobalOptions, setGlobalContext } from '@osskit/monitor';
import  logger from './logger.js';

setGlobalOptions({
  context: { globalContextId: 'bla' },
  logResult: true,
  logExecutionStart: false,
  parseError: (res) => console.log(res),
  prometheusBuckets: [0.0001, 0.1, 0.5, 10],
  logger,
});

setGlobalContext(() => getDynamicContext());
```

## API
### createMonitor({ scope: string, options?: MonitorOptions })
#### scope
Type: `string`

The scope of the monitor's metrics

Returns an instance of a function that calls `monitor` - `<T>(method: string, callable: () => T, options?: MonitorOptions<T>)`

### monitor(method: string, callable: () => T, options?: MonitorOptions)
#### method
Type: `string`

#### MonitorOptions
`{ context?: Record<string, any>;
  logResult?: boolean;
  logExecutionStart?: boolean;
  parseResult?: (res: any) => any;
  parseError?: (e: any) => any; 
  }`
  #### GlobalOptions
`{ context?: Record<string, any>;
  logResult?: boolean;
  logExecutionStart?: boolean;
  parseError?: (e: any) => any;
  prometheusBuckets?: number[];
  logger?: BaseLogger; // pino's BaseLogger instance
  }`
  
  ## License
  MIT License
