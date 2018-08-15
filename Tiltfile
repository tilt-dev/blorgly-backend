def blorgly_backend_local():
  return blorgly_backend('devel')

def blorgly_backend_prod():
  return blorgly_backend('prod')

def blorgly_backend(env):
  entrypoint = '/go/bin/blorgly-backend'
  image = build_docker_image('Dockerfile.base', 'gcr.io/blorg-dev/blorgly-backend:' + env + '-' + local('whoami').rstrip('\n'), entrypoint)
  image.add_mount('/go/src/github.com/windmilleng/blorgly-backend', git_repo('.'))
  image.add_cmd('go install github.com/windmilleng/blorgly-backend')
  # print(image)
  yaml = local('python populate_config_template.py ' + env + ' 1>&2 && cat k8s-conf.generated.yaml')

  # this api might be cleaner than stderr stuff above
  # run('python populate_config_template.py ' + env')
  # yaml = read('k8s-conf.generated.yaml')

  # print(yaml)
  return k8s_service(yaml, image)
