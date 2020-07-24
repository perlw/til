# EC2: Attaching Volumes to Instances via CloudFormation

### As part of an Instance definition

This creates the `Volume` as part of an `Instance`'s life-cycle and will create and
tear it down (unless `DeleteOnTermination` property is false) together with the
parent `Instance`.

```yaml
MyInstance:
  Type: AWS::EC2::Instance
  Properties:
    BlockDeviceMappings:
      - DeviceName: /dev/sdf
        Ebs:
          VolumeSize: 1024 # GB
          VolumeType: st1 # gp2, io1, sc1, st1, standard
          DeleteOnTermination: true # True by default on root volumes, false on
                                    # non-root volumes.
```

### Stand-alone with direct link via Volume property

This creates a stand-alone `Volume` which is not part of the `Instance` life-cycle
though it remains a dependency for `Instance` creation. The problem that
remains is picking the `AvailabilityZone` since you might like it to be chosen
automatically in the `Instance`.

```yaml
MyVolume:
  Type: AWS::EC2::Volume
  Properties:
    AvailabilityZone: eu-west-1a # Must be the same Zone for both Volume and
                                 # Instance.
    Size: 1024 # GB
    VolumeType: st1 # gp2, io1, sc1, st1, standard

MyInstance:
  Type: AWS::EC2::Instance
  Properties:
    AvailabilityZone: eu-west-1a # Must be the same Zone for both Volume and
                                 # Instance.
    Volumes:
      - Device: /dev/sdf
        VolumeId: !Ref MyVolume
```

### Stand-alone with a VolumeAttachment

This creates a stand-alone `Volume` which is not part of the `Instance` life-cycle.
It is not a direct dependency on `Instance` creation but instead attached later.
This enables setting the `AvailabilityZone` from the `Instance`'s
`AvailabilityZone` enabling it to be set completely automatically.

```yaml
MyVolume:
  Type: AWS::EC2::Volume
  Properties:
    AvailabilityZone: !GetAtt MyInstance.AvailabilityZone
    Size: 1024 # GB
    VolumeType: st1 # gp2, io1, sc1, st1, standard

MyInstance:
  Type: AWS::EC2::Instance
  Properties:

MyVolumeAttachment:
  Type: AWS::EC2::VolumeAttachment
  Properties:
    Device: /dev/sdf
    InstanceId: !Ref MyInstance
    VolumeId: !Ref MyVolume
```

---

Sources: [EC2:Instance](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-ec2-instance.html) | [EC2:Volume](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-ec2-ebs-volume.html) | [EC2:VolumeAttachment](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-ec2-ebs-volumeattachment.html)
