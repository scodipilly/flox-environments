# Local task-runner for validating Flox environments — not a k8s dev stack.
# Tiltfile

##############################################
# Configuration
##############################################

update_settings(max_parallel_updates=4)
os.putenv('FLOX_DISABLE_METRICS', 'true')

##############################################
# Discover Flox environments
##############################################

environments = str(local(
    'find . -path "*/.flox/env/manifest.toml" '
    '| sed -E "s#^./([^/]+)/.flox/env/manifest.toml$#\\1#" '
    '| sort -u',
    quiet=True,
    echo_off=True,
)).strip().split('\n')

##############################################
# Build + validate each environment
##############################################

for env in environments:
    if not env:
        continue

    local_resource(
        'build-' + env,
        cmd='flox activate --dir ' + env + ' -- true',
        deps=[
            env + '/.flox/env/manifest.toml',
            env + '/.flox/env/manifest.lock',
        ],
        labels=['environments'],
    )
