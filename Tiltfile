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

raw_paths = str(local(
    'find . -path "*/.flox/env/manifest.toml"',
    quiet=True,
    echo_off=True,
)).strip().split('\n')
environments = [p.split('/')[1] for p in raw_paths if p]

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
