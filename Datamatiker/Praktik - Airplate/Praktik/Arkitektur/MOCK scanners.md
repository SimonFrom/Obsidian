import type { scanner } from '../airplateTypes/CompanyDetails';

function makeScanner(overrides: Partial<scanner> = {}): scanner {
  return {
    IMEI: '352709570858738',
    name: 'Unit',
    last_seen: '2026-08-11 10:50:00',
    charging: 0,
    batteryVoltage: 4.0,
    latitude: 56.13,
    longitude: 10.27,
    enable_auto_update: 0,
    version: '1.0',
    ...overrides,
  };
}

const scannersMOCK: scanner[] = [
  makeScanner({ IMEI: 'A' }),
  makeScanner({ IMEI: 'B', GNSSDisabledFlag: 1 }),
  makeScanner({ group: 'West', last_seen: '2026-08-11 10:50:00' }),
  makeScanner({ group: 'West', last_seen: '2026-08-10 09:50:00' }),
  makeScanner({ group: 'East', last_seen: '2026-08-10 09:50:00' }),
];