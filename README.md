[![Create Release][release-badge]][release-url]
[![Import Labels][import-labels-badge]][import-labels-url]
[![Sync Labels][sync-labels-badge]][sync-labels-url]
[![PR AutoLabeler][autolabeler-badge]][autolabeler-url]
[![Assigner][assigner-badge]][assigner-url]

Description
Composite action combining common steps preceding stack deployment or removal for deploy/deprovision workflows

### Inputs
#### `region`
Region to deploy to/remove from
required: true
default: `${{ matrix.region }}`

#### `stage`
The Stage being deployed/removed
required: true
default: `${{ matrix.stage }}`

#### `service`
The Service being deployed/removed
required: true

#### `action`
The specified command (deploy/remove).
required: false
default: `deploy`

#### `stack_policy_stages`
Comma delimited list of stages to apply stack policy against.  This ensures that stack policy is not
applied against undesired stacks.
required: false
default: "dev,prod,master"

#### `stack_policy_file`
Path to an optional stack policy file to be applied (deployment only!)
required: false

### Example usage
#### deployment
```yaml
      - name: serverless deploy
        id: deploy
        uses: CumulusDS/sls-deploy-remove-action@v1
        with:
          service: ${{ steps.role.outputs.service }}
          stack_policy_file: "templates/policies/serverless.json"
```

#### removal
```yaml
      - name: Set feature stage
        id: set-feature-stage
        uses: CumulusDS/feature-stage-action@v1
      - name: serverless remove
        id: remove
        uses: CumulusDS/sls-deploy-remove-action@v1
        with:
          service: ${{ steps.role.outputs.service }}
          stage: ${{ steps.set-feature-stage.outputs.stage }}
          action: remove
```

The results can be later referenced again for use in a separate step if desired using the `results` output from the step.
In order to reference the `result` output, you must assign and `id` to the step for future referencing.

[release-badge]: https://github.com/CumulusDS/sls-deploy-remove-action/actions/workflows/release.yml/badge.svg
[release-url]: https://github.com/CumulusDS/sls-deploy-remove-action/actions/workflows/release.yml
[import-labels-badge]: https://github.com/CumulusDS/sls-deploy-remove-action/actions/workflows/labels_import.yml/badge.svg
[import-labels-url]: https://github.com/CumulusDS/sls-deploy-remove-action/actions/workflows/labels_import.yml
[sync-labels-badge]: https://github.com/CumulusDS/sls-deploy-remove-action/actions/workflows/labels_sync.yml/badge.svg
[sync-labels-url]: https://github.com/CumulusDS/sls-deploy-remove-action/actions/workflows/labels_sync.yml
[autolabeler-badge]: https://github.com/CumulusDS/sls-deploy-remove-action/actions/workflows/autolabeler.yml/badge.svg
[autolabeler-url]: https://github.com/CumulusDS/sls-deploy-remove-action/actions/workflows/autolabeler.yml
[assigner-badge]: https://github.com/CumulusDS/sls-deploy-remove-action/actions/workflows/assign.yml/badge.svg
[assigner-url]: https://github.com/CumulusDS/sls-deploy-remove-action/actions/workflows/assign.yml
