def blorgly_backend_local():
  return blorgly_backend('devel')

def blorgly_backend_prod():
  return blorgly_backend('prod')

def blorgly_backend(env):
  entrypoint = '/app/server'
  image = build_docker_image('Dockerfile.base', 'gcr.io/blorg-dev/blorgly-backend:' + env + '-' + local('whoami').rstrip('\n'), entrypoint)
  src_dir = '/go/src/github.com/windmilleng/blorgly-backend'
  image.add(src_dir, git_repo('.'))
  image.run('cd ' + src_dir + '; go get ./...')
  image.run('mkdir -p /app')
  image.run('cd ' + src_dir + '; go build -o server; cp server /app/')
  # print(image)
  yaml = local('python populate_config_template.py ' + env + ' 1>&2 && cat k8s-conf.generated.yaml')

  # this api might be cleaner than stderr stuff above
  # run('python populate_config_template.py ' + env')
  # yaml = read('k8s-conf.generated.yaml')

  # print(yaml)
  return k8s_service(yaml, image)
