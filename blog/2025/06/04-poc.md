# Poc (2025-06-04)

## pocs/README.md

---
description: Proof-of-Concepts
---

# PoC examples

 * [DHCP](dhcp_example/README.md)
 * [DS](stb_ds_example/README.md)
 * [XMLDocBook](xml_docbook_insights/README.md)
 * [Postgres LTREE](ai_studio_pg_ltree/README.md)
 * [OTP 29 rc3 tests](otp29/README.md)





Included file: by-date/2025/06/04-poc_assets/Makefile

## pocs/dhcp_example/README.md


# Implementation of DHCP-model

Many examples exist on how to model network equipment, and many need
a data-structure in C to fill these models by using an API.

This folder contains an example of such an implementation.

We use the [stb](https://github.com/nothings/stb)-library.

## Specifics

The implementation assumed fixed string-length.

See the dhcp_config.c file for this.

On many embedded systems we know (and must know) the limitations
which are 

## Contents

```yang
module dhcpd {
  namespace "http://modeling.com/example/dhcpd";
  prefix dhcpd;

  import ietf-inet-types {
    prefix inet;
  }

  typedef loglevel {
    type enumeration {
      enum "kern";
      enum "mail";
      enum "local7";
    }
  }

  container dhcp {
    leaf default-lease-time {
      type xs:duration;
      default "PT600S";
    }
    leaf max-lease-time {
      type xs:duration;
      default "PT7200S";
    }
    leaf log-facility {
      type loglevel;
      default "local7";
    }
    container subnets {
      list subnet {
        key "net mask";
        leaf net {
          type inet:ipv4-address;
        }
        leaf mask {
          type inet:ipv4-address;
        }
        container range {
          presence "";
          leaf dynamic-bootp {
            description
              "Enable BOOTP for this instance.";
            type boolean;
            default "false";
          }
          leaf low-addr {
            description
              "Enable BOOTP for this instance.";
            mandatory true;
            type inet:ipv4-address;
          }
          leaf high-addr {
            description
              "Enable BOOTP for this instance.";
            type inet:ipv4-address;
          }
        }
        leaf routers {
          type string;
        }
        leaf max-lease-time {
          type xs:duration;
          default "PT7200S";
        }
      }
    }
    container shared-networks {
      list shared-network {
        max-elements 1024;
        key name;
        leaf name {
          type string;
        }
        container subnets {
          list subnet {
            key "net mask";
            leaf net {
              type inet:ipv4-address;
            }
            leaf mask {
              type inet:ipv4-address;
            }
            container range {
              presence "";
              leaf dynamic-bootp {
                description
                  "Enable BOOTP for this instance.";
                type boolean;
                default "false";
              }
              leaf low-addr {
                description
                  "Enable BOOTP for this instance.";
                mandatory true;
                type inet:ipv4-address;
              }
              leaf high-addr {
                description
                  "Enable BOOTP for this instance.";
                type inet:ipv4-address;
              }
            }
            leaf routers {
              type string;
            }
            leaf max-lease-time {
              type string;
              default "PT7200S";
            }
          }
        }
      }
    }
  }
}

```

```makefile
# Makefile for STB Data Structures example
TARGET = dhcp_config

STB_URL = https://raw.githubusercontent.com/nothings/stb/master/stb_ds.h
STB_HEADER = stb_ds.h

STB_SURL = https://raw.githubusercontent.com/nothings/stb/master/stb_sprintf.h
STB_SHEADER = stb_sprintf.h

.PHONY: all clean deps

all: deps $(TARGET)

# Download stb_ds.h if missing
deps:
	@if [ ! -f "$(STB_HEADER)" ]; then \
		echo "Downloading $(STB_HEADER)..."; \
		curl -s -o $(STB_HEADER) $(STB_URL) || wget -q -O $(STB_HEADER) $(STB_URL); \
	fi
	@if [ ! -f "$(STB_SHEADER)" ]; then \
		echo "Downloading $(STB_SHEADER)..."; \
		curl -s -o $(STB_SHEADER) $(STB_SURL) || wget -q -O $(STB_SHEADER) $(STB_SURL); \
	fi

# Compile the program
$(TARGET): $(TARGET).c
	$(CC) -O2 -Wall -Wextra -o $@ $<

clean:
	$(RM) $(TARGET)

distclean: clean
	$(RM) $(STB_HEADER)
	

```

```c
#include <stdio.h>
#include <stdbool.h>
#include <string.h>

#define STB_DS_IMPLEMENTATION
#include "stb_ds.h"

#define STRING_LENGTH_MAX 32

#define STRCPY(dest, src) do { \
    static_assert(sizeof(dest) <= STRING_LENGTH_MAX, "Buffer overflow risk"); \
    strncpy((dest), (src), sizeof(dest)-1); \
    (dest)[sizeof(dest)-1] = '\0'; \
} while(0)

// --------------------------
// Type Definitions
// --------------------------

typedef enum {
    LOG_KERN,
    LOG_MAIL,
    LOG_LOCAL7
} LogFacility;

typedef struct {
    char low_addr[STRING_LENGTH_MAX];
    char high_addr[STRING_LENGTH_MAX];
    bool dynamic_bootp;
} DhcpRange;

typedef struct {
    char net[STRING_LENGTH_MAX];
    char mask[STRING_LENGTH_MAX];
    DhcpRange range;  // No longer a pointer
    bool has_range;   // Flag to indicate presence
    char routers[STRING_LENGTH_MAX];
    char max_lease_time[STRING_LENGTH_MAX];
} DhcpSubnet;

typedef struct {
    char name[STRING_LENGTH_MAX];
    DhcpSubnet* subnets;
} DhcpSharedNetwork;

typedef struct {
    char default_lease_time[STRING_LENGTH_MAX];
    char max_lease_time[STRING_LENGTH_MAX];
    LogFacility log_facility;
    
    struct {
        DhcpSubnet* subnets;
    } subnets;
    
    struct {
        DhcpSharedNetwork* networks;
    } shared_networks;
} DhcpConfig;

// --------------------------
// Helper Functions
// --------------------------

void init_subnet(DhcpSubnet* subnet, const char* net, const char* mask) {
    memset(subnet, 0, sizeof(DhcpSubnet));
    STRCPY(subnet->net, net);
    STRCPY(subnet->mask, mask);
    STRCPY(subnet->max_lease_time, "PT7200S");
    subnet->has_range = false;
}

void set_range(DhcpSubnet* subnet, const char* low, const char* high, bool dynamic_bootp) {
    STRCPY(subnet->range.low_addr, low);
    if (high) {
        STRCPY(subnet->range.high_addr, high);
    }
    subnet->range.dynamic_bootp = dynamic_bootp;
    subnet->has_range = true;
}

void free_dhcp_config(DhcpConfig* config) {
    if (!config) return;
    arrfree(config->subnets.subnets);
    
    for (int i = 0; i < arrlen(config->shared_networks.networks); i++) {
        arrfree(config->shared_networks.networks[i].subnets);
    }
    arrfree(config->shared_networks.networks);
}

// --------------------------
// Main Program
// --------------------------

void print_config(const DhcpConfig* config) {
    printf("DHCP Configuration:\n");
    printf("  Default Lease Time: %s\n", config->default_lease_time);
    printf("  Max Lease Time: %s\n", config->max_lease_time);
    printf("  Log Facility: %d\n", config->log_facility);
    
    printf("\nSubnets:\n");
    for (int i = 0; i < arrlen(config->subnets.subnets); i++) {
        const DhcpSubnet* s = &config->subnets.subnets[i];
        printf("  - %s/%s\n", s->net, s->mask);
        if (s->has_range) {
            printf("    Range: %s-%s (BOOTP: %s)\n", 
                   s->range.low_addr,
                   s->range.high_addr[0] ? s->range.high_addr : "N/A",
                   s->range.dynamic_bootp ? "enabled" : "disabled");
        }
    }
    
    printf("\nShared Networks:\n");
    for (int i = 0; i < arrlen(config->shared_networks.networks); i++) {
        const DhcpSharedNetwork* n = &config->shared_networks.networks[i];
        printf("  Network: %s\n", n->name);
        for (int j = 0; j < arrlen(n->subnets); j++) {
            const DhcpSubnet* s = &n->subnets[j];
            printf("    - %s/%s\n", s->net, s->mask);
        }
    }
}

int main() {
    DhcpConfig config = {0};
    
    // Initialize fixed-size strings
    STRCPY(config.default_lease_time, "PT600S");
    STRCPY(config.max_lease_time, "PT7200S");
    config.log_facility = LOG_LOCAL7;
    
    // Add regular subnets
    DhcpSubnet subnet1;
    init_subnet(&subnet1, "192.168.1.0", "255.255.255.0");
    STRCPY(subnet1.routers, "192.168.1.1");
    set_range(&subnet1, "192.168.1.100", "192.168.1.200", false);
    arrput(config.subnets.subnets, subnet1);
    
    // Add shared network
    DhcpSharedNetwork shared = {0};
    STRCPY(shared.name, "Office-Network");
    
    DhcpSubnet shared_subnet1;
    init_subnet(&shared_subnet1, "10.0.1.0", "255.255.255.0");
    set_range(&shared_subnet1, "10.0.1.50", "10.0.1.150", true);
    arrput(shared.subnets, shared_subnet1);

    DhcpSubnet shared_subnet2;
    init_subnet(&shared_subnet2, "192.168.15.0", "255.255.255.0");
    set_range(&shared_subnet2, "192.168.15.50", "192.168.15.150", true);
    arrput(shared.subnets, shared_subnet2);
    
    arrput(config.shared_networks.networks, shared);
    
    print_config(&config);
    free_dhcp_config(&config);
    return 0;
}

```

## Download links

{% file src="dhcpd.yang" %}dhcpd.yang{% endfile %}
{% file src="Makefile" %}Makefile{% endfile %}
{% file src="dhcp_config.c" %}dhcp_config.c{% endfile %}



Included file: by-date/2025/06/04-poc_assets/dhcp_config.c

Included file: by-date/2025/06/04-poc_assets/dhcp_config.c.charptr

Included file: by-date/2025/06/04-poc_assets/dhcpd.yang

Included file: by-date/2025/06/04-poc_assets/stb_ds.h

Included file: by-date/2025/06/04-poc_assets/stb_sprintf.h

Included file: by-date/2025/06/04-poc_assets/Makefile

## pocs/stb_ds_example/README.md



# Implementation of an inteface-list

Many examples exist on how to model network equipment, and many need
a data-structure in C to fill these models by using an API.

This folder contains an example on how to structure a list of interfaces.

We use the [stb](https://github.com/nothings/stb)-library.


## Contents

```makefile
# Makefile for STB Data Structures example
TARGET = network_interfaces
STB_URL = https://raw.githubusercontent.com/nothings/stb/master/stb_ds.h
STB_HEADER = stb_ds.h

STB_SURL = https://raw.githubusercontent.com/nothings/stb/master/stb_sprintf.h
STB_SHEADER = stb_sprintf.h

.PHONY: all clean deps

all: deps $(TARGET)

# Download stb_ds.h if missing
deps:
	@if [ ! -f "$(STB_HEADER)" ]; then \
		echo "Downloading $(STB_HEADER)..."; \
		curl -s -o $(STB_HEADER) $(STB_URL) || wget -q -O $(STB_HEADER) $(STB_URL); \
	fi
	@if [ ! -f "$(STB_SHEADER)" ]; then \
		echo "Downloading $(STB_SHEADER)..."; \
		curl -s -o $(STB_SHEADER) $(STB_SURL) || wget -q -O $(STB_SHEADER) $(STB_SURL); \
	fi

# Compile the program
$(TARGET): main.c
	$(CC) -std=c99 -O2 -Wall -Wextra -o $@ $<

# Compile the program
string_test: string_test.c
	$(CC) -std=c99 -O2 -Wall -Wextra -o $@ $<

clean:
	$(RM) $(TARGET)

distclean: clean
	$(RM) $(STB_HEADER)
	

```

```c
#define STB_DS_IMPLEMENTATION
#include "stb_ds.h"
#include <stdio.h>
#include <string.h>

// --- Interface Status Enum ---
typedef enum {
    INTERFACE_UP = 1,       // Ready to pass packets
    INTERFACE_DOWN = 2,     // Not ready + not in test mode
    INTERFACE_TESTING = 3   // In test mode
} InterfaceStatus;

const char* status_to_string(InterfaceStatus s) {
    static const char* strings[] = {
        [INTERFACE_UP]      = "UP (Ready to pass packets)",
        [INTERFACE_DOWN]    = "DOWN (Not ready)",
        [INTERFACE_TESTING] = "TESTING (In test mode)"
    };
    return strings[s];
}

// --- Data Structures ---
typedef struct {
    int id;
    int vlan;
    char type[32];  // "access", "trunk", etc.
} SubInterface;

typedef struct {
    char name[64];
    char desc[128];
    InterfaceStatus status;
    SubInterface* subinterfaces; // stb_ds dynamic array
} NetworkInterface;

// --- Core Functions ---
void add_subinterface(NetworkInterface* intf, int id, int vlan, const char* type) {
    SubInterface sub = {.id = id, .vlan = vlan};
    strncpy(sub.type, type, sizeof(sub.type)-1);
    arrput(intf->subinterfaces, sub);
}

void clear_subinterfaces(NetworkInterface* intf) {
    if (intf && intf->subinterfaces) {
        arrfree(intf->subinterfaces);
        intf->subinterfaces = NULL;
    }
}

void remove_subinterfaces_by_vlan(NetworkInterface* intf, int vlan) {
    if (!intf || !intf->subinterfaces) return;
    
    SubInterface* filtered = NULL;
    for (int i = 0; i < arrlen(intf->subinterfaces); i++) {
        if (intf->subinterfaces[i].vlan != vlan) {
            arrput(filtered, intf->subinterfaces[i]);
        }
    }
    
    arrfree(intf->subinterfaces);
    intf->subinterfaces = filtered;
}

void print_interface(const NetworkInterface* intf) {
    printf("┌─ %s [%s]\n│  Status: %s\n│  Desc: %s\n", 
           intf->name, 
           arrlen(intf->subinterfaces) ? "Composite" : "Simple",
           status_to_string(intf->status),
           intf->desc);
    
    for (int i = 0; i < arrlen(intf->subinterfaces); i++) {
        printf("├── Subif %d: VLAN %d (%s)\n", 
               intf->subinterfaces[i].id,
               intf->subinterfaces[i].vlan,
               intf->subinterfaces[i].type);
    }
    printf("└────────────────\n");
}

// --- Main Program ---
int main() {
    printf("=== Network Interface Demo ===\n\n");
    
    // Create interface with subinterfaces
    NetworkInterface eth0 = {
        .name = "eth0", 
        .desc = "Main Ethernet port",
        .status = INTERFACE_UP
    };
    add_subinterface(&eth0, 1, 100, "access");
    add_subinterface(&eth0, 2, 200, "trunk");
    add_subinterface(&eth0, 3, 100, "hybrid");
    
    // Create wireless interface
    NetworkInterface wlan0 = {
        .name = "wlan0",
        .desc = "Wireless interface (testing mode)",
        .status = INTERFACE_TESTING
    };
    add_subinterface(&wlan0, 1, 300, "access");
    
    // Print initial state
    printf("Initial interfaces:\n");
    print_interface(&eth0);
    print_interface(&wlan0);
    
    // Demo VLAN removal
    printf("\nRemoving VLAN 100 from eth0...\n");
    remove_subinterfaces_by_vlan(&eth0, 100);
    print_interface(&eth0);
    
    // Demo clear
    printf("\nClearing all wlan0 subinterfaces...\n");
    clear_subinterfaces(&wlan0);
    print_interface(&wlan0);
    
    // Cleanup
    clear_subinterfaces(&eth0);
    clear_subinterfaces(&wlan0);
    
    return 0;
}
```

```c
#define STB_SPRINTF_IMPLEMENTATION
#include "stb_sprintf.h"  // Single header version
#include <stdio.h>

int main() {
    char buffer[100];
    
    // 1. Basic formatting (like sprintf)
    stbsp_sprintf(buffer, "Hello, %s! You have %d messages.", "User", 5);
    puts(buffer);  // Output: "Hello, User! You have 5 messages."

    // 2. Safer bounds checking
    stbsp_snprintf(buffer, sizeof(buffer), "Pi ≈ %.5f", 3.1415926535);
    puts(buffer);  // Output: "Pi ≈ 3.14159"

    // 3. Advanced features (no standard library equivalent)
    stbsp_sprintf(buffer, "Hex: %08x | Binary: %b", 255, 255);
    puts(buffer);  // Output: "Hex: 000000ff | Binary: 11111111"

    // 4. Custom formatting (no allocations)
    stbsp_sprintf(buffer, "Commas: %'d", 1000000);
    puts(buffer);  // Output: "Commas: 1,000,000"
    
    return 0;
}

```

## Download links

{% file src="Makefile" %}Makefile{% endfile %}
{% file src="main.c" %}main.c{% endfile %}
{% file src="string_test.c" %}string_test.c{% endfile %}


Included file: by-date/2025/06/04-poc_assets/main.c

Included file: by-date/2025/06/04-poc_assets/stb_ds.h

Included file: by-date/2025/06/04-poc_assets/stb_sprintf.h

Included file: by-date/2025/06/04-poc_assets/string_test.c

## pocs/xml_docbook_insights/README.md

# XML and DocBook

I have always been interrested in docbook. Never had to time to really
get into it, however, for some legacy systems the documentation is still
in docbook and currently there are no plans to get away from it.

So; let us take the opertunity to get to know a bit more, jotting down 
so insight and snipplets which are used along the way.

## Entity resolution

One thing I always liked is templating, and the simplest forms are the
best. Before jumping into a full-fledged tool based on python or lua and
template engines such as mustache etc, there must be simpler ways of injecting
variables into XML-documents.

### xmllint to the rescue

Let us take the following template as example, co-located variables at the
top for easy adaptation:

```xml
<!DOCTYPE config [
    <!ENTITY HOST "example.com">
    <!ENTITY PORT "8080">
    <!ENTITY PROTOCOL "HTTPS">
]>
<config>
    <server>
        <address>http://&HOST;:&PORT;</address>
        <protocol>&PROTOCOL;</protocol>
    </server>
</config>
```
To expand it run:

```bash
$ xmllint --noent config-co.xml.in --dropdtd > config.xml
```

The result:

```xml
<?xml version="1.0"?>
<config>
    <server>
        <address>http://example.com:8080</address>
        <protocol>HTTPS</protocol>
    </server>
</config>
```



Using a seperate file: vars.env

```bash
<!ENTITY HOST "example.com">
<!ENTITY PORT "8080">
<!ENTITY PROTOCOL "HTTPS">
```

Given the following template:

```xml
<!DOCTYPE config SYSTEM "vars.env">
<config>
    <server>
        <address>http://&HOST;:&PORT;</address>
        <protocol>&PROTOCOL;</protocol>
    </server>
</config>
```

Invoking xmllint: 
```bash
xmllint --noent --loaddtd input-env.xml.in --dropdtd > input-env.xml
```

Results in:

```xml
<?xml version="1.0"?>
<config>
    <server>
        <address>http://example.com:8080</address>
        <protocol>HTTPS</protocol>
    </server>
</config>
```

#### Take aways

- Simple, no need for any libraries - other than xmllint.
- Standard, no need for sed-voodoo magic ( eventhough this also works!)
- Validation
  - The file must be valid syntax
  - Proper error-messages, for instance if vars.env does not have PORT-entitiy:
```
input-env.xml.in:4: parser error : Entity 'PORT' not defined
        <address>http://&HOST;:&PORT;</address>
```

Included file: by-date/2025/06/04-poc_assets/config-co.xml

Included file: by-date/2025/06/04-poc_assets/config-co.xml.in

Included file: by-date/2025/06/04-poc_assets/input-env.xml

Included file: by-date/2025/06/04-poc_assets/input-env.xml.in

Included file: by-date/2025/06/04-poc_assets/vars.env

